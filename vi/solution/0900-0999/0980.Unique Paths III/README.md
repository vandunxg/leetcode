---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - Backtracking
    - Hamiltonian Path
    - Matrix
---

<!-- problem:start -->

# [980. Unique Paths III](https://leetcode.com/problems/unique-paths-iii)

[中文文档](/solution/0900-0999/0980.Unique%20Paths%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>m x n</code> <code>grid</code>, trong đó <code>grid[i][j]</code> có thể là:</p>

<ul>
	<li><code>1</code> biểu thị ô bắt đầu. Có đúng một ô bắt đầu.</li>
	<li><code>2</code> biểu thị ô kết thúc. Có đúng một ô kết thúc.</li>
	<li><code>0</code> biểu thị ô trống có thể đi qua.</li>
	<li><code>-1</code> biểu thị chướng ngại vật không thể đi qua.</li>
</ul>

<p>Hãy trả về <em>số đường đi từ ô bắt đầu đến ô kết thúc theo bốn hướng, sao cho mỗi ô không phải chướng ngại vật được đi qua đúng một lần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0980.Unique%20Paths%20III/images/lc-unique1.jpg" style="width: 324px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có hai đường đi như sau: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0980.Unique%20Paths%20III/images/lc-unique2.jpg" style="width: 324px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0],[0,0,0,0],[0,0,0,2]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có bốn đường đi như sau: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0980.Unique%20Paths%20III/images/lc-unique3-.jpg" style="width: 164px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1],[2,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có đường đi nào đi qua mỗi ô trống đúng một lần.
Lưu ý, ô bắt đầu và ô kết thúc có thể nằm ở bất kỳ vị trí nào trong lưới.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 20</code></li>
	<li><code>1 &lt;= m * n &lt;= 20</code></li>
	<li><code>-1 &lt;= grid[i][j] &lt;= 2</code></li>
	<li>Có đúng một ô bắt đầu và một ô kết thúc.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Backtracking

<!-- thinking:start -->

> **Tư duy**
>
> Đi từ điểm bắt đầu đến điểm kết thúc, ghé qua mỗi ô trống đúng một lần. Lưới có tối đa $20$ ô nên có thể dùng backtracking. Đếm số ô trống và tìm điểm bắt đầu, sau đó DFS theo bốn hướng với set đánh dấu ô đã thăm. Chỉ tính đường đi đến ô kết thúc khi số bước bằng số ô trống cộng một.

<!-- thinking:end -->

Trước tiên, duyệt toàn bộ lưới để tìm điểm bắt đầu $(x, y)$ và đếm số ô trống $cnt$.

Tiếp theo, bắt đầu tìm kiếm từ điểm xuất phát để đếm tất cả đường đi. Ta định nghĩa hàm $dfs(i, j, k)$, trong đó $(i, j)$ là vị trí hiện tại và $k$ là số bước đã đi.

Trong hàm, trước tiên kiểm tra ô hiện tại có phải điểm kết thúc hay không. Nếu đúng, kiểm tra $k$ có bằng $cnt + 1$ không. Nếu bằng, đường đi hiện tại hợp lệ và trả về $1$; nếu không thì trả về $0$.

Nếu ô hiện tại không phải điểm kết thúc, xét bốn ô kề với nó. Với mỗi ô kề chưa được thăm, đánh dấu ô đó đã thăm rồi tiếp tục tìm kiếm từ ô ấy. Sau khi tìm kiếm xong, bỏ đánh dấu ô kề. Cuối cùng, trả về tổng số đường đi từ tất cả các ô kề.

Cuối cùng, trả về số đường đi từ điểm xuất phát, tức $dfs(x, y, 1)$.

Độ phức tạp thời gian là $O(3^{m \times n})$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniquePathsIII(self, grid: List[List[int]]) -> int:
        def dfs(i: int, j: int, k: int) -> int:
            if grid[i][j] == 2:
                return int(k == cnt + 1)
            ans = 0
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and (x, y) not in vis and grid[x][y] != -1:
                    vis.add((x, y))
                    ans += dfs(x, y, k + 1)
                    vis.remove((x, y))
            return ans

        m, n = len(grid), len(grid[0])
        start = next((i, j) for i in range(m) for j in range(n) if grid[i][j] == 1)
        dirs = (-1, 0, 1, 0, -1)
        cnt = sum(row.count(0) for row in grid)
        vis = {start}
        return dfs(*start, 0)
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int cnt;
    private int[][] grid;
    private boolean[][] vis;

    public int uniquePathsIII(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        int x = 0, y = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    ++cnt;
                } else if (grid[i][j] == 1) {
                    x = i;
                    y = j;
                }
            }
        }
        vis = new boolean[m][n];
        vis[x][y] = true;
        return dfs(x, y, 0);
    }

    private int dfs(int i, int j, int k) {
        if (grid[i][j] == 2) {
            return k == cnt + 1 ? 1 : 0;
        }
        int ans = 0;
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int h = 0; h < 4; ++h) {
            int x = i + dirs[h], y = j + dirs[h + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && grid[x][y] != -1) {
                vis[x][y] = true;
                ans += dfs(x, y, k + 1);
                vis[x][y] = false;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int uniquePathsIII(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int cnt = 0;
        for (auto& row : grid) {
            for (auto& x : row) {
                cnt += x == 0;
            }
        }
        int dirs[5] = {-1, 0, 1, 0, -1};
        bool vis[m][n];
        memset(vis, false, sizeof vis);
        function<int(int, int, int)> dfs = [&](int i, int j, int k) -> int {
            if (grid[i][j] == 2) {
                return k == cnt + 1 ? 1 : 0;
            }
            int ans = 0;
            for (int h = 0; h < 4; ++h) {
                int x = i + dirs[h], y = j + dirs[h + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && grid[x][y] != -1) {
                    vis[x][y] = true;
                    ans += dfs(x, y, k + 1);
                    vis[x][y] = false;
                }
            }
            return ans;
        };
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    vis[i][j] = true;
                    return dfs(i, j, 0);
                }
            }
        }
        return 0;
    }
};
```

#### Go

```go
func uniquePathsIII(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	cnt := 0
	vis := make([][]bool, m)
	x, y := 0, 0
	for i, row := range grid {
		vis[i] = make([]bool, n)
		for j, v := range row {
			if v == 0 {
				cnt++
			} else if v == 1 {
				x, y = i, j
			}
		}
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if grid[i][j] == 2 {
			if k == cnt+1 {
				return 1
			}
			return 0
		}
		ans := 0
		for h := 0; h < 4; h++ {
			x, y := i+dirs[h], j+dirs[h+1]
			if x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && grid[x][y] != -1 {
				vis[x][y] = true
				ans += dfs(x, y, k+1)
				vis[x][y] = false
			}
		}
		return ans
	}
	vis[x][y] = true
	return dfs(x, y, 0)
}
```

#### TypeScript

```ts
function uniquePathsIII(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    let [x, y] = [0, 0];
    let cnt = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 0) {
                ++cnt;
            } else if (grid[i][j] == 1) {
                [x, y] = [i, j];
            }
        }
    }
    const vis: boolean[][] = Array.from({ length: m }, () => Array(n).fill(false));
    vis[x][y] = true;
    const dirs = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number, k: number): number => {
        if (grid[i][j] === 2) {
            return k === cnt + 1 ? 1 : 0;
        }
        let ans = 0;
        for (let d = 0; d < 4; ++d) {
            const [x, y] = [i + dirs[d], j + dirs[d + 1]];
            if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && grid[x][y] !== -1) {
                vis[x][y] = true;
                ans += dfs(x, y, k + 1);
                vis[x][y] = false;
            }
        }
        return ans;
    };
    return dfs(x, y, 0);
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var uniquePathsIII = function (grid) {
    const m = grid.length;
    const n = grid[0].length;
    let [x, y] = [0, 0];
    let cnt = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 0) {
                ++cnt;
            } else if (grid[i][j] === 1) {
                [x, y] = [i, j];
            }
        }
    }
    const vis = Array.from({ length: m }, () => Array(n).fill(false));
    vis[x][y] = true;
    const dirs = [-1, 0, 1, 0, -1];
    const dfs = function (i, j, k) {
        if (grid[i][j] === 2) {
            return k === cnt + 1 ? 1 : 0;
        }
        let ans = 0;
        for (let d = 0; d < 4; ++d) {
            const x = i + dirs[d];
            const y = j + dirs[d + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && grid[x][y] !== -1) {
                vis[x][y] = true;
                ans += dfs(x, y, k + 1);
                vis[x][y] = false;
            }
        }
        return ans;
    };
    return dfs(x, y, 0);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
