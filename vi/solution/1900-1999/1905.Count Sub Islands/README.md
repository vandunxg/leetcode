---
comments: true
difficulty: Medium
rating: 1678
source: Weekly Contest 246 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Matrix
---

<!-- problem:start -->

# [1905. Count Sub Islands](https://leetcode.com/problems/count-sub-islands)

[中文文档](/solution/1900-1999/1905.Count%20Sub%20Islands/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai ma trận nhị phân <code>m x n</code> là <code>grid1</code> và <code>grid2</code>, chỉ chứa <code>0</code> (biểu diễn nước) và <code>1</code> (biểu diễn đất). Một <strong>hòn đảo</strong> là một nhóm các ô <code>1</code> được nối với nhau theo <strong>4 hướng</strong> (ngang hoặc dọc). Mọi ô nằm ngoài ma trận đều được xem là ô nước.</p>

<p>Một hòn đảo trong <code>grid2</code> được xem là <strong>hòn đảo con</strong> nếu có một hòn đảo trong <code>grid1</code> chứa <strong>tất cả</strong> các ô tạo nên <strong>hòn đảo này</strong> trong <code>grid2</code>.</p>

<p>Trả về <em><strong>số lượng</strong> các hòn đảo trong </em><code>grid2</code> <em>được xem là <strong>hòn đảo con</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1905.Count%20Sub%20Islands/images/test1.png" style="width: 493px; height: 205px;" />
<pre>
<strong>Đầu vào:</strong> grid1 = [[1,1,1,0,0],[0,1,1,1,1],[0,0,0,0,0],[1,0,0,0,0],[1,1,0,1,1]], grid2 = [[1,1,1,0,0],[0,0,1,1,1],[0,1,0,0,0],[1,0,1,1,0],[0,1,0,1,0]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Trong hình trên, lưới bên trái là grid1 và lưới bên phải là grid2.
Các ô 1 được tô đỏ trong grid2 là những ô thuộc các hòn đảo con. Có ba hòn đảo con.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1905.Count%20Sub%20Islands/images/testcasex2.png" style="width: 491px; height: 201px;" />
<pre>
<strong>Đầu vào:</strong> grid1 = [[1,0,1,0,1],[1,1,1,1,1],[0,0,0,0,0],[1,1,1,1,1],[1,0,1,0,1]], grid2 = [[0,0,0,0,0],[1,1,1,1,1],[0,1,0,1,0],[0,1,0,1,0],[1,0,0,0,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Trong hình trên, lưới bên trái là grid1 và lưới bên phải là grid2.
Các ô 1 được tô đỏ trong grid2 là những ô thuộc các hòn đảo con. Có hai hòn đảo con.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid1.length == grid2.length</code></li>
	<li><code>n == grid1[i].length == grid2[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>grid1[i][j]</code> và <code>grid2[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hòn đảo của $\textit{grid2}$ chỉ là hòn đảo con khi mọi ô đất của nó cũng là ô đất trong $\textit{grid1}$. Kiểm tra từng ô riêng lẻ không thể gom chúng theo từng hòn đảo.
>
> Một DFS (hoặc BFS) duyệt một thành phần, đặt các ô của thành phần đó trong $\textit{grid2}$ thành 0 để đánh dấu đã thăm, đồng thời AND các ô tương ứng trong $\textit{grid1}$ để xác định hòn đảo có hợp lệ hay không.
>
> Ta duyệt toàn bộ ma trận, bắt đầu tìm kiếm tại mỗi ô $1$ còn lại trong $\textit{grid2}$ rồi cộng các giá trị trả về.

<!-- thinking:end -->

Ta có thể duyệt từng ô $(i, j)$ trong ma trận `grid2`. Nếu giá trị của ô là $1$, ta bắt đầu tìm kiếm theo chiều sâu từ ô này, đặt giá trị của tất cả các ô nối với nó thành $0$, đồng thời ghi nhận liệu ô tương ứng trong `grid1` có là $1$ đối với mọi ô thuộc thành phần đó hay không. Nếu là $1$, điều đó có nghĩa là thành phần này cũng là một hòn đảo trong `grid1`, ngược lại thì không. Cuối cùng, ta đếm số hòn đảo con trong `grid2`.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của hai ma trận `grid1` và `grid2`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubIslands(self, grid1: List[List[int]], grid2: List[List[int]]) -> int:
        def dfs(i: int, j: int) -> int:
            ok = grid1[i][j]
            grid2[i][j] = 0
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and grid2[x][y] and not dfs(x, y):
                    ok = 0
            return ok

        m, n = len(grid1), len(grid1[0])
        dirs = (-1, 0, 1, 0, -1)
        return sum(dfs(i, j) for i in range(m) for j in range(n) if grid2[i][j])
```

#### Java

```java
class Solution {
    private final int[] dirs = {-1, 0, 1, 0, -1};
    private int[][] grid1;
    private int[][] grid2;
    private int m;
    private int n;

    public int countSubIslands(int[][] grid1, int[][] grid2) {
        m = grid1.length;
        n = grid1[0].length;
        this.grid1 = grid1;
        this.grid2 = grid2;
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid2[i][j] == 1) {
                    ans += dfs(i, j);
                }
            }
        }
        return ans;
    }

    private int dfs(int i, int j) {
        int ok = grid1[i][j];
        grid2[i][j] = 0;
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid2[x][y] == 1) {
                ok &= dfs(x, y);
            }
        }
        return ok;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSubIslands(vector<vector<int>>& grid1, vector<vector<int>>& grid2) {
        int m = grid1.size(), n = grid1[0].size();
        int ans = 0;
        int dirs[5] = {-1, 0, 1, 0, -1};
        function<int(int, int)> dfs = [&](int i, int j) {
            int ok = grid1[i][j];
            grid2[i][j] = 0;
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && grid2[x][y]) {
                    ok &= dfs(x, y);
                }
            }
            return ok;
        };
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid2[i][j]) {
                    ans += dfs(i, j);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSubIslands(grid1 [][]int, grid2 [][]int) (ans int) {
	m, n := len(grid1), len(grid1[0])
	dirs := [5]int{-1, 0, 1, 0, -1}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		ok := grid1[i][j]
		grid2[i][j] = 0
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && grid2[x][y] == 1 && dfs(x, y) == 0 {
				ok = 0
			}
		}
		return ok
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if grid2[i][j] == 1 {
				ans += dfs(i, j)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countSubIslands(grid1: number[][], grid2: number[][]): number {
    const [m, n] = [grid1.length, grid1[0].length];
    let ans = 0;
    const dirs: number[] = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number): number => {
        let ok = grid1[i][j];
        grid2[i][j] = 0;
        for (let k = 0; k < 4; ++k) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < m && y >= 0 && y < n && grid2[x][y]) {
                ok &= dfs(x, y);
            }
        }
        return ok;
    };
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; j++) {
            if (grid2[i][j]) {
                ans += dfs(i, j);
            }
        }
    }
    return ans;
}
```

#### JavaScript

```js
function countSubIslands(grid1, grid2) {
    const [m, n] = [grid1.length, grid1[0].length];
    let ans = 0;
    const dirs = [-1, 0, 1, 0, -1];
    const dfs = (i, j) => {
        let ok = grid1[i][j];
        grid2[i][j] = 0;
        for (let k = 0; k < 4; ++k) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < m && y >= 0 && y < n && grid2[x][y]) {
                ok &= dfs(x, y);
            }
        }
        return ok;
    };
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; j++) {
            if (grid2[i][j]) {
                ans += dfs(i, j);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
