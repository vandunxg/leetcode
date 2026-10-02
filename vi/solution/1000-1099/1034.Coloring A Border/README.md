---
comments: true
difficulty: Medium
rating: 1578
source: Weekly Contest 134 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [1034. Coloring A Border](https://leetcode.com/problems/coloring-a-border)

[中文文档](/solution/1000-1099/1034.Coloring%20A%20Border/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ma trận số nguyên <code>m x n</code> <code>grid</code> và ba số nguyên <code>row</code>, <code>col</code>, <code>color</code>. Mỗi giá trị trong ma trận biểu thị màu của ô tại vị trí đó.</p>

<p>Hai ô được gọi là <strong>kề nhau</strong> nếu chúng nằm cạnh nhau theo một trong 4 hướng.</p>

<p>Hai ô thuộc cùng một <strong>thành phần liên thông</strong> nếu chúng có cùng màu và kề nhau.</p>

<p><strong>Đường biên của một thành phần liên thông</strong> gồm tất cả các ô trong thành phần đó có ít nhất một ô kề không thuộc thành phần, hoặc nằm trên biên của ma trận (hàng đầu, hàng cuối, cột đầu hoặc cột cuối).</p>

<p>Hãy tô <strong>đường biên</strong> của <strong>thành phần liên thông</strong> chứa ô <code>grid[row][col]</code> bằng màu <code>color</code>.</p>

<p>Trả về <em>ma trận sau cùng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> grid = [[1,1],[1,2]], row = 0, col = 0, color = 3
<strong>Output:</strong> [[3,3],[3,2]]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> grid = [[1,2,2],[2,3,2]], row = 0, col = 1, color = 3
<strong>Output:</strong> [[1,3,3],[2,3,3]]
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Input:</strong> grid = [[1,1,1],[1,1,1],[1,1,1]], row = 1, col = 1, color = 2
<strong>Output:</strong> [[2,2,2],[2,1,2],[2,2,2]]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>1 &lt;= grid[i][j], color &lt;= 1000</code></li>
	<li><code>0 &lt;= row &lt; m</code></li>
	<li><code>0 &lt;= col &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tìm thành phần liên thông chứa $(row,col)$ rồi xác định ô nào nằm trên đường biên. Vì $m,n\le 50$, chỉ cần duyệt một lần. Một ô biên nằm ở rìa ma trận hoặc có ô kề khác màu.
>
> DFS duyệt thành phần liên thông. Với mỗi ô, nếu có ô kề nằm ngoài ma trận hoặc khác màu thì ô hiện tại thuộc đường biên và được tô thành $\textit{color}$. Ma trận đánh dấu đã thăm giúp tránh duyệt lại ô.
>
> Bắt đầu từ ô đã cho để hoàn tất việc tô lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def colorBorder(
        self, grid: List[List[int]], row: int, col: int, color: int
    ) -> List[List[int]]:
        def dfs(i: int, j: int, c: int) -> None:
            vis[i][j] = True
            for a, b in pairwise((-1, 0, 1, 0, -1)):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n:
                    if not vis[x][y]:
                        if grid[x][y] == c:
                            dfs(x, y, c)
                        else:
                            grid[i][j] = color
                else:
                    grid[i][j] = color

        m, n = len(grid), len(grid[0])
        vis = [[False] * n for _ in range(m)]
        dfs(row, col, grid[row][col])
        return grid
```

#### Java

```java
class Solution {
    private int[][] grid;
    private int color;
    private int m;
    private int n;
    private boolean[][] vis;

    public int[][] colorBorder(int[][] grid, int row, int col, int color) {
        this.grid = grid;
        this.color = color;
        m = grid.length;
        n = grid[0].length;
        vis = new boolean[m][n];
        dfs(row, col, grid[row][col]);
        return grid;
    }

    private void dfs(int i, int j, int c) {
        vis[i][j] = true;
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n) {
                if (!vis[x][y]) {
                    if (grid[x][y] == c) {
                        dfs(x, y, c);
                    } else {
                        grid[i][j] = color;
                    }
                }
            } else {
                grid[i][j] = color;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> colorBorder(vector<vector<int>>& grid, int row, int col, int color) {
        int m = grid.size();
        int n = grid[0].size();
        bool vis[m][n];
        memset(vis, false, sizeof(vis));
        int dirs[5] = {-1, 0, 1, 0, -1};
        function<void(int, int, int)> dfs = [&](int i, int j, int c) {
            vis[i][j] = true;
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k];
                int y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n) {
                    if (!vis[x][y]) {
                        if (grid[x][y] == c) {
                            dfs(x, y, c);
                        } else {
                            grid[i][j] = color;
                        }
                    }
                } else {
                    grid[i][j] = color;
                }
            }
        };
        dfs(row, col, grid[row][col]);
        return grid;
    }
};
```

#### Go

```go
func colorBorder(grid [][]int, row int, col int, color int) [][]int {
	m, n := len(grid), len(grid[0])
	vis := make([][]bool, m)
	for i := range vis {
		vis[i] = make([]bool, n)
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	var dfs func(int, int, int)
	dfs = func(i, j, c int) {
		vis[i][j] = true
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n {
				if !vis[x][y] {
					if grid[x][y] == c {
						dfs(x, y, c)
					} else {
						grid[i][j] = color
					}
				}
			} else {
				grid[i][j] = color
			}
		}
	}
	dfs(row, col, grid[row][col])
	return grid
}
```

#### TypeScript

```ts
function colorBorder(grid: number[][], row: number, col: number, color: number): number[][] {
    const m = grid.length;
    const n = grid[0].length;
    const vis = new Array(m).fill(0).map(() => new Array(n).fill(false));
    const dirs = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number, c: number) => {
        vis[i][j] = true;
        for (let k = 0; k < 4; ++k) {
            const x = i + dirs[k];
            const y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n) {
                if (!vis[x][y]) {
                    if (grid[x][y] == c) {
                        dfs(x, y, c);
                    } else {
                        grid[i][j] = color;
                    }
                }
            } else {
                grid[i][j] = color;
            }
        }
    };
    dfs(row, col, grid[row][col]);
    return grid;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
