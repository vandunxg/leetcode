---
comments: true
difficulty: Medium
rating: 1461
source: Biweekly Contest 161 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Matrix
---

<!-- problem:start -->

# [3619. Count Islands With Total Value Divisible by K](https://leetcode.com/problems/count-islands-with-total-value-divisible-by-k)

[中文文档](/solution/3600-3699/3619.Count%20Islands%20With%20Total%20Value%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <code>m x n</code> <code>grid</code> và một số nguyên dương <code>k</code>. Một <strong>hòn đảo</strong> là một nhóm các số nguyên <strong>dương</strong> (đại diện cho đất) được kết nối theo <strong>4 hướng</strong> (theo chiều ngang hoặc chiều dọc).</p>

<p><strong>Tổng giá trị</strong> của một hòn đảo là tổng giá trị của tất cả các ô trong hòn đảo đó.</p>

<p>Trả về số lượng hòn đảo có tổng giá trị <strong>chia hết cho</strong> <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3619.Count%20Islands%20With%20Total%20Value%20Divisible%20by%20K/images/example1griddrawio-1.png" style="width: 200px; height: 200px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,2,1,0,0],[0,5,0,0,5],[0,0,1,0,0],[0,1,4,7,0],[0,2,0,0,8]], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ma trận có bốn hòn đảo. Tổng giá trị của các hòn đảo được tô màu xanh dương chia hết cho 5, còn tổng giá trị của các hòn đảo được tô màu đỏ thì không.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3619.Count%20Islands%20With%20Total%20Value%20Divisible%20by%20K/images/example2griddrawio.png" style="width: 200px; height: 150px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[3,0,3,0], [0,3,0,3], [3,0,3,0]], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ma trận có sáu hòn đảo, mỗi hòn đảo có tổng giá trị chia hết cho 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>m == grid.length</code></li>
    <li><code>n == grid[i].length</code></li>
    <li><code>1 &lt;= m, n &lt;= 1000</code></li>
    <li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= grid[i][j] &lt;= 10<sup>6</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Một hòn đảo là một thành phần liên thông theo 4 hướng của các ô dương. Ta cần phần dư của tổng giá trị mỗi hòn đảo khi chia cho $k$, không cần biết hình dạng của nó. DFS cộng dồn tổng đồng thời gán các ô đã thăm về 0, nhờ đó mỗi ô không bị duyệt lại.
>
> Ta duyệt grid và bắt đầu $\textit{dfs}$ từ mọi ô vẫn còn dương; tăng đáp án khi tổng trả về chia hết cho $k$. Mảng offset gộp $(-1,0,1,0,-1)$ tạo ra bốn ô kề.
>
> Mỗi ô được đi vào một lần, nên thời gian thực thi tương ứng với kích thước của ma trận.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(i, j)$ thực hiện duyệt DFS bắt đầu từ vị trí $(i, j)$ và trả về tổng giá trị của hòn đảo đó. Ta cộng giá trị của vị trí hiện tại vào tổng, sau đó đánh dấu vị trí này đã được thăm (chẳng hạn bằng cách đặt giá trị của nó thành 0). Tiếp theo, ta đệ quy thăm các vị trí kề theo bốn hướng (lên, xuống, trái, phải). Nếu một vị trí kề có giá trị lớn hơn 0, ta tiếp tục DFS và cộng giá trị của nó vào tổng. Cuối cùng, ta trả về tổng giá trị.

Trong hàm chính, ta duyệt toàn bộ ma trận. Với mỗi vị trí $(i, j)$ chưa được thăm, nếu giá trị của nó lớn hơn 0, ta gọi $\textit{dfs}(i, j)$ để tính tổng giá trị của hòn đảo đó. Nếu tổng giá trị chia hết cho $k$, ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countIslands(self, grid: List[List[int]], k: int) -> int:
        def dfs(i: int, j: int) -> int:
            s = grid[i][j]
            grid[i][j] = 0
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and grid[x][y]:
                    s += dfs(x, y)
            return s

        m, n = len(grid), len(grid[0])
        dirs = (-1, 0, 1, 0, -1)
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] and dfs(i, j) % k == 0:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] grid;
    private final int[] dirs = {-1, 0, 1, 0, -1};

    public int countIslands(int[][] grid, int k) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] > 0 && dfs(i, j) % k == 0) {
                    ++ans;
                }
            }
        }
        return ans;
    }

    private long dfs(int i, int j) {
        long s = grid[i][j];
        grid[i][j] = 0;
        for (int d = 0; d < 4; ++d) {
            int x = i + dirs[d], y = j + dirs[d + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] > 0) {
                s += dfs(x, y);
            }
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countIslands(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        vector<int> dirs = {-1, 0, 1, 0, -1};

        auto dfs = [&](this auto&& dfs, int i, int j) -> long long {
            long long s = grid[i][j];
            grid[i][j] = 0;
            for (int d = 0; d < 4; ++d) {
                int x = i + dirs[d], y = j + dirs[d + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y]) {
                    s += dfs(x, y);
                }
            }
            return s;
        };

        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] && dfs(i, j) % k == 0) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countIslands(grid [][]int, k int) (ans int) {
    m, n := len(grid), len(grid[0])
    dirs := []int{-1, 0, 1, 0, -1}
    var dfs func(i, j int) int
    dfs = func(i, j int) int {
        s := grid[i][j]
        grid[i][j] = 0
        for d := 0; d < 4; d++ {
            x, y := i+dirs[d], j+dirs[d+1]
            if x >= 0 && x < m && y >= 0 && y < n && grid[x][y] > 0 {
                s += dfs(x, y)
            }
        }
        return s
    }
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] > 0 && dfs(i, j)%k == 0 {
                ans++
            }
        }
    }
    return
}
```

#### TypeScript

```ts
function countIslands(grid: number[][], k: number): number {
    const m = grid.length,
        n = grid[0].length;
    const dirs = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number): number => {
        let s = grid[i][j];
        grid[i][j] = 0;
        for (let d = 0; d < 4; d++) {
            const x = i + dirs[d],
                y = j + dirs[d + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] > 0) {
                s += dfs(x, y);
            }
        }
        return s;
    };

    let ans = 0;
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] > 0 && dfs(i, j) % k === 0) {
                ans++;
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
