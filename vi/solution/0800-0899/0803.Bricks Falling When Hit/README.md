---
comments: true
difficulty: Hard
tags:
    - Union Find
    - Array
    - Matrix
---

<!-- problem:start -->

# [803. Bricks Falling When Hit](https://leetcode.com/problems/bricks-falling-when-hit)

[中文文档](/solution/0800-0899/0803.Bricks%20Falling%20When%20Hit/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>grid</code> nhị phân kích thước <code>m x n</code>, trong đó mỗi <code>1</code> biểu thị một viên gạch và <code>0</code> biểu thị ô trống. Một viên gạch được xem là <strong>ổn định</strong> nếu:</p>

<ul>
	<li>Nó được nối trực tiếp với cạnh trên của lưới, hoặc</li>
	<li>Ít nhất một viên gạch khác trong bốn ô kề với nó <strong>ổn định</strong>.</li>
</ul>

<p>Bạn cũng được cho mảng <code>hits</code>, là chuỗi các thao tác xóa cần thực hiện. Mỗi lần, ta xóa viên gạch tại vị trí <code>hits[i] = (row<sub>i</sub>, col<sub>i</sub>)</code>. Viên gạch ở vị trí đó (nếu có) sẽ biến mất. Một số viên gạch khác có thể mất tính ổn định do thao tác xóa này và sẽ <strong>rơi</strong>. Khi rơi, viên gạch sẽ bị xóa <strong>ngay lập tức</strong> khỏi <code>grid</code> (tức là nó không rơi xuống các viên gạch ổn định khác).</p>

<p>Hãy trả về <em>mảng </em><code>result</code><em>, trong đó mỗi </em><code>result[i]</code><em> là số viên gạch sẽ <strong>rơi</strong> sau khi thực hiện thao tác xóa thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p><strong>Lưu ý</strong> rằng thao tác xóa có thể trỏ đến vị trí không có viên gạch; khi đó sẽ không có viên nào rơi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0],[1,1,1,0]], hits = [[1,0]]
<strong>Đầu ra:</strong> [2]
<strong>Giải thích: </strong>Bắt đầu với lưới:
[[1,0,0,0],
 [<u>1</u>,1,1,0]]
Ta xóa viên gạch được gạch chân tại (1,0), lưới thu được là:
[[1,0,0,0],
 [0,<u>1</u>,<u>1</u>,0]]
Hai viên gạch được gạch chân không còn ổn định vì chúng không còn nối với cạnh trên và cũng không kề với viên gạch ổn định nào khác, nên chúng sẽ rơi. Lưới thu được là:
[[1,0,0,0],
 [0,0,0,0]]
Vì vậy, kết quả là [2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0],[1,1,0,0]], hits = [[1,1],[1,0]]
<strong>Đầu ra:</strong> [0,0]
<strong>Giải thích: </strong>Bắt đầu với lưới:
[[1,0,0,0],
 [1,<u>1</u>,0,0]]
Ta xóa viên gạch được gạch chân tại (1,1), lưới thu được là:
[[1,0,0,0],
 [1,0,0,0]]
Các viên gạch còn lại vẫn ổn định, nên không có viên nào rơi. Lưới giữ nguyên:
[[1,0,0,0],
 [<u>1</u>,0,0,0]]
Tiếp theo, ta xóa viên gạch được gạch chân tại (1,0), lưới thu được là:
[[1,0,0,0],
 [0,0,0,0]]
Một lần nữa, các viên gạch còn lại vẫn ổn định, nên không có viên nào rơi.
Vì vậy, kết quả là [0,0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>grid[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>1 &lt;= hits.length &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>hits[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i&nbsp;</sub>&lt;= m - 1</code></li>
	<li><code>0 &lt;=&nbsp;y<sub>i</sub> &lt;= n - 1</code></li>
	<li>Mọi <code>(x<sub>i</sub>, y<sub>i</sub>)</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Flood-fill sau mỗi lần xóa sẽ quá chậm với $4\cdot 10^4$ thao tác trên lưới $200\times 200$. Một viên gạch rơi khi và chỉ khi nó mất kết nối với hàng trên cùng.
>
> Trước tiên, xóa tất cả vị trí bị tác động, nối các viên gạch còn lại với một mái ảo bằng Union-Find, rồi khôi phục các thao tác theo thứ tự ngược lại. Mức tăng số viên trong thành phần liên thông chứa mái, trừ đi một, là số viên gạch đáng lẽ đã rơi; thao tác xóa ô trống đóng góp bằng không.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hitBricks(self, grid: List[List[int]], hits: List[List[int]]) -> List[int]:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        def union(a, b):
            pa, pb = find(a), find(b)
            if pa != pb:
                size[pb] += size[pa]
                p[pa] = pb

        m, n = len(grid), len(grid[0])
        p = list(range(m * n + 1))
        size = [1] * len(p)
        g = deepcopy(grid)
        for i, j in hits:
            g[i][j] = 0
        for j in range(n):
            if g[0][j] == 1:
                union(j, m * n)
        for i in range(1, m):
            for j in range(n):
                if g[i][j] == 0:
                    continue
                if g[i - 1][j] == 1:
                    union(i * n + j, (i - 1) * n + j)
                if j > 0 and g[i][j - 1] == 1:
                    union(i * n + j, i * n + j - 1)
        ans = []
        for i, j in hits[::-1]:
            if grid[i][j] == 0:
                ans.append(0)
                continue
            g[i][j] = 1
            prev = size[find(m * n)]
            if i == 0:
                union(j, m * n)
            for a, b in [(-1, 0), (1, 0), (0, 1), (0, -1)]:
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and g[x][y] == 1:
                    union(i * n + j, x * n + y)
            curr = size[find(m * n)]
            ans.append(max(0, curr - prev - 1))
        return ans[::-1]
```

#### Java

```java
class Solution {
    private int[] p;
    private int[] size;

    public int[] hitBricks(int[][] grid, int[][] hits) {
        int m = grid.length;
        int n = grid[0].length;
        p = new int[m * n + 1];
        size = new int[m * n + 1];
        for (int i = 0; i < p.length; ++i) {
            p[i] = i;
            size[i] = 1;
        }
        int[][] g = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                g[i][j] = grid[i][j];
            }
        }
        for (int[] h : hits) {
            g[h[0]][h[1]] = 0;
        }
        for (int j = 0; j < n; ++j) {
            if (g[0][j] == 1) {
                union(j, m * n);
            }
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (g[i][j] == 0) {
                    continue;
                }
                if (g[i - 1][j] == 1) {
                    union(i * n + j, (i - 1) * n + j);
                }
                if (j > 0 && g[i][j - 1] == 1) {
                    union(i * n + j, i * n + j - 1);
                }
            }
        }
        int[] ans = new int[hits.length];
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int k = hits.length - 1; k >= 0; --k) {
            int i = hits[k][0];
            int j = hits[k][1];
            if (grid[i][j] == 0) {
                continue;
            }
            g[i][j] = 1;
            int prev = size[find(m * n)];
            if (i == 0) {
                union(j, m * n);
            }
            for (int l = 0; l < 4; ++l) {
                int x = i + dirs[l];
                int y = j + dirs[l + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && g[x][y] == 1) {
                    union(i * n + j, x * n + y);
                }
            }
            int curr = size[find(m * n)];
            ans[k] = Math.max(0, curr - prev - 1);
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    private void union(int a, int b) {
        int pa = find(a);
        int pb = find(b);
        if (pa != pb) {
            size[pb] += size[pa];
            p[pa] = pb;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> p;
    vector<int> size;

    vector<int> hitBricks(vector<vector<int>>& grid, vector<vector<int>>& hits) {
        int m = grid.size(), n = grid[0].size();
        p.resize(m * n + 1);
        size.resize(m * n + 1);
        for (int i = 0; i < p.size(); ++i) {
            p[i] = i;
            size[i] = 1;
        }
        vector<vector<int>> g(m, vector<int>(n));
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                g[i][j] = grid[i][j];
        for (auto& h : hits) g[h[0]][h[1]] = 0;
        for (int j = 0; j < n; ++j)
            if (g[0][j] == 1)
                merge(j, m * n);
        for (int i = 1; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (g[i][j] == 0) continue;
                if (g[i - 1][j] == 1) merge(i * n + j, (i - 1) * n + j);
                if (j > 0 && g[i][j - 1] == 1) merge(i * n + j, i * n + j - 1);
            }
        }
        vector<int> ans(hits.size());
        vector<int> dirs = {-1, 0, 1, 0, -1};
        for (int k = hits.size() - 1; k >= 0; --k) {
            int i = hits[k][0], j = hits[k][1];
            if (grid[i][j] == 0) continue;
            g[i][j] = 1;
            int prev = size[find(m * n)];
            if (i == 0) merge(j, m * n);
            for (int l = 0; l < 4; ++l) {
                int x = i + dirs[l], y = j + dirs[l + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && g[x][y] == 1)
                    merge(i * n + j, x * n + y);
            }
            int curr = size[find(m * n)];
            ans[k] = max(0, curr - prev - 1);
        }
        return ans;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    void merge(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa != pb) {
            size[pb] += size[pa];
            p[pa] = pb;
        }
    }
};
```

#### Go

```go
func hitBricks(grid [][]int, hits [][]int) []int {
	m, n := len(grid), len(grid[0])
	p := make([]int, m*n+1)
	size := make([]int, len(p))
	for i := range p {
		p[i] = i
		size[i] = 1
	}

	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	union := func(a, b int) {
		pa, pb := find(a), find(b)
		if pa != pb {
			size[pb] += size[pa]
			p[pa] = pb
		}
	}

	g := make([][]int, m)
	for i := range g {
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = grid[i][j]
		}
	}
	for _, h := range hits {
		g[h[0]][h[1]] = 0
	}
	for j, v := range g[0] {
		if v == 1 {
			union(j, m*n)
		}
	}
	for i := 1; i < m; i++ {
		for j := 0; j < n; j++ {
			if g[i][j] == 0 {
				continue
			}
			if g[i-1][j] == 1 {
				union(i*n+j, (i-1)*n+j)
			}
			if j > 0 && g[i][j-1] == 1 {
				union(i*n+j, i*n+j-1)
			}
		}
	}
	ans := make([]int, len(hits))
	dirs := []int{-1, 0, 1, 0, -1}
	for k := len(hits) - 1; k >= 0; k-- {
		i, j := hits[k][0], hits[k][1]
		if grid[i][j] == 0 {
			continue
		}
		g[i][j] = 1
		prev := size[find(m*n)]
		if i == 0 {
			union(j, m*n)
		}
		for l := 0; l < 4; l++ {
			x, y := i+dirs[l], j+dirs[l+1]
			if x >= 0 && x < m && y >= 0 && y < n && g[x][y] == 1 {
				union(i*n+j, x*n+y)
			}
		}
		curr := size[find(m*n)]
		ans[k] = max(0, curr-prev-1)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
