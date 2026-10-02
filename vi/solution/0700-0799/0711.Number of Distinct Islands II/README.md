---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Hash Table
    - Matrix
    - Sorting
    - Hash Function
---

<!-- problem:start -->

# [711. Number of Distinct Islands II 🔒](https://leetcode.com/problems/number-of-distinct-islands-ii)

[中文文档](/solution/0700-0799/0711.Number%20of%20Distinct%20Islands%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>grid</code>. Một hòn đảo là nhóm các ô <code>1</code> (đại diện cho đất) kết nối với nhau theo <strong>4 hướng</strong> (ngang hoặc dọc). Có thể giả sử cả bốn cạnh của grid đều được bao quanh bởi nước.</p>

<p>Hai hòn đảo được xem là giống nhau nếu chúng có cùng hình dạng hoặc trùng nhau sau khi <b>xoay</b> (chỉ 90, 180 hoặc 270 độ) hay <b>phản chiếu</b> (theo chiều trái/phải hoặc trên/dưới).</p>

<p>Trả về <em>số lượng hòn đảo <b>khác nhau</b></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0711.Number%20of%20Distinct%20Islands%20II/images/distinctisland2-1-grid.jpg" style="width: 413px; height: 334px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0,0,0],[1,0,0,0,0],[0,0,0,0,1],[0,0,0,1,1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Hai hòn đảo được xem là giống nhau vì nếu xoay hòn đảo đầu tiên 180 độ theo chiều kim đồng hồ, hai hòn đảo sẽ có cùng hình dạng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0711.Number%20of%20Distinct%20Islands%20II/images/distinctisland1-1-grid.jpg" style="width: 413px; height: 334px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0,0,0],[1,1,0,0,0],[0,0,0,1,1],[0,0,0,1,1]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các hòn đảo kết nối theo 4 hướng, xem như tương đương nếu xoay hoặc phản chiếu được. Grid có kích thước tối đa $50\times 50$, nên có thể tách từng đảo rồi so sánh hình dạng.
>
> Bài Number of Distinct Islands I chỉ cần chuẩn hóa hình dạng theo phép tịnh tiến. Ở đây, nhóm đối xứng dihedral gộp tám hướng khác nhau, nên ta tạo cả tám biến thể, đưa mỗi biến thể về cùng gốc tọa độ rồi chọn biến thể nhỏ nhất theo thứ tự từ điển.
>
> DFS thu thập các ô của đảo; mỗi $(i,j)$ tạo ra các tọa độ $(\pm i,\pm j)$ và $(\pm j,\pm i)$. Sắp xếp từng biến thể, trừ tọa độ điểm đầu tiên để chuẩn hóa, rồi thêm tuple nhỏ nhất trong tám tuple vào set. Kích thước set là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numDistinctIslands2(self, grid: List[List[int]]) -> int:
        def dfs(i, j, shape):
            shape.append([i, j])
            grid[i][j] = 0
            for a, b in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and grid[x][y] == 1:
                    dfs(x, y, shape)

        def normalize(shape):
            shapes = [[] for _ in range(8)]
            for i, j in shape:
                shapes[0].append([i, j])
                shapes[1].append([i, -j])
                shapes[2].append([-i, j])
                shapes[3].append([-i, -j])
                shapes[4].append([j, i])
                shapes[5].append([j, -i])
                shapes[6].append([-j, i])
                shapes[7].append([-j, -i])
            for e in shapes:
                e.sort()
                for i in range(len(e) - 1, -1, -1):
                    e[i][0] -= e[0][0]
                    e[i][1] -= e[0][1]
            shapes.sort()
            return tuple(tuple(e) for e in shapes[0])

        m, n = len(grid), len(grid[0])
        s = set()
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    shape = []
                    dfs(i, j, shape)
                    s.add(normalize(shape))
        return len(s)
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] grid;

    public int numDistinctIslands2(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        Set<List<List<Integer>>> s = new HashSet<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    List<Integer> shape = new ArrayList<>();
                    dfs(i, j, shape);
                    s.add(normalize(shape));
                }
            }
        }
        return s.size();
    }

    private List<List<Integer>> normalize(List<Integer> shape) {
        List<int[]>[] shapes = new List[8];
        for (int i = 0; i < 8; ++i) {
            shapes[i] = new ArrayList<>();
        }
        for (int e : shape) {
            int i = e / n;
            int j = e % n;
            shapes[0].add(new int[] {i, j});
            shapes[1].add(new int[] {i, -j});
            shapes[2].add(new int[] {-i, j});
            shapes[3].add(new int[] {-i, -j});
            shapes[4].add(new int[] {j, i});
            shapes[5].add(new int[] {j, -i});
            shapes[6].add(new int[] {-j, i});
            shapes[7].add(new int[] {-j, -i});
        }
        for (List<int[]> e : shapes) {
            e.sort((a, b) -> {
                int i1 = a[0];
                int j1 = a[1];
                int i2 = b[0];
                int j2 = b[1];
                if (i1 == i2) {
                    return j1 - j2;
                }
                return i1 - i2;
            });
            int a = e.get(0)[0];
            int b = e.get(0)[1];
            for (int k = e.size() - 1; k >= 0; --k) {
                int i = e.get(k)[0];
                int j = e.get(k)[1];
                e.set(k, new int[] {i - a, j - b});
            }
        }
        Arrays.sort(shapes, (a, b) -> {
            for (int k = 0; k < a.size(); ++k) {
                int i1 = a.get(k)[0];
                int j1 = a.get(k)[1];
                int i2 = b.get(k)[0];
                int j2 = b.get(k)[1];
                if (i1 != i2) {
                    return i1 - i2;
                }
                if (j1 != j2) {
                    return j1 - j2;
                }
            }
            return 0;
        });
        List<List<Integer>> ans = new ArrayList<>();
        for (int[] e : shapes[0]) {
            ans.add(Arrays.asList(e[0], e[1]));
        }
        return ans;
    }

    private void dfs(int i, int j, List<Integer> shape) {
        shape.add(i * n + j);
        grid[i][j] = 0;
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k];
            int y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                dfs(x, y, shape);
            }
        }
    }
}
```

#### C++

```cpp
typedef pair<int, int> PII;

class Solution {
public:
    int numDistinctIslands2(vector<vector<int>>& grid) {
        set<vector<PII>> s;
        for (int i = 0; i < grid.size(); ++i) {
            for (int j = 0; j < grid[0].size(); ++j) {
                if (grid[i][j]) {
                    vector<PII> shape;
                    dfs(i, j, grid, shape);
                    s.insert(normalize(shape));
                }
            }
        }
        return s.size();
    }

    vector<PII> normalize(vector<PII>& shape) {
        vector<vector<PII>> shapes(8);
        for (auto& e : shape) {
            int i = e.first, j = e.second;
            shapes[0].push_back({i, j});
            shapes[1].push_back({i, -j});
            shapes[2].push_back({-i, j});
            shapes[3].push_back({-i, -j});
            shapes[4].push_back({j, i});
            shapes[5].push_back({j, -i});
            shapes[6].push_back({-j, -i});
            shapes[7].push_back({-j, i});
        }
        for (auto& e : shapes) {
            sort(e.begin(), e.end());
            for (int k = e.size() - 1; k >= 0; --k) {
                e[k].first -= e[0].first;
                e[k].second -= e[0].second;
            }
        }
        sort(shapes.begin(), shapes.end());
        return shapes[0];
    }

    void dfs(int i, int j, vector<vector<int>>& grid, vector<PII>& shape) {
        shape.push_back({i, j});
        grid[i][j] = 0;
        vector<int> dirs = {-1, 0, 1, 0, -1};
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < grid.size() && y >= 0 && y < grid[0].size() && grid[x][y] == 1)
                dfs(x, y, grid, shape);
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
