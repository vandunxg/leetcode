---
comments: true
difficulty: Hard
rating: 2208
source: Weekly Contest 204 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
    - Articulation Point
    - Strongly Connected Component
---

<!-- problem:start -->

# [1568. Minimum Number of Days to Disconnect Island](https://leetcode.com/problems/minimum-number-of-days-to-disconnect-island)

[中文文档](/solution/1500-1599/1568.Minimum%20Number%20of%20Days%20to%20Disconnect%20Island/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>grid</code>, trong đó <code>1</code> là đất và <code>0</code> là nước. Một <strong>đảo</strong> là một nhóm các số <code>1</code> liên thông <strong>theo 4 hướng</strong> (ngang hoặc dọc) tối đại.</p>

<p>Ma trận được gọi là <strong>liên thông</strong> nếu có <strong>đúng một đảo</strong>, ngược lại gọi là <strong>không liên thông</strong>.</p>

<p>Trong một ngày, ta được phép đổi <strong>bất kỳ </strong>một ô đất <code>(1)</code> thành ô nước <code>(0)</code>.</p>

<p>Trả về <em>số ngày ít nhất để làm ma trận không liên thông</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1568.Minimum%20Number%20of%20Days%20to%20Disconnect%20Island/images/land1.jpg" style="width: 500px; height: 169px;" />
<pre>
<strong>Input:</strong> grid = [[0,1,1,0],[0,1,1,0],[0,0,0,0]]

<strong>Output:</strong> 2
<strong>Giải thích:</strong> Cần ít nhất 2 ngày để có ma trận không liên thông.
Đổi grid[1][1] và grid[0][2] từ đất thành nước, ta được 2 đảo không liên thông.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1568.Minimum%20Number%20of%20Days%20to%20Disconnect%20Island/images/land2.jpg" style="width: 404px; height: 85px;" />
<pre>
<strong>Input:</strong> grid = [[1,1]]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Ma trận toàn nước cũng là không liên thông ([[1,1]] -&gt; [[0,0]]), có 0 đảo.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 30</code></li>
	<li><code>grid[i][j]</code> is either <code>0</code> or <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày có thể đổi một ô đất thành nước; ta muốn ma trận không còn đúng một đảo. Ma trận nhỏ, và có một tính chất mạnh hơn: mọi đảo đều có thể bị tách bằng cách xóa nhiều nhất hai ô (chẳng hạn hai ô đất ở góc kề nhau).
>
> Vì vậy đáp án chỉ có thể là $0$, $1$ hoặc $2$. Nếu số đảo đếm bằng DFS khác 1, trả về $0$. Tạm thời xóa từng ô đất và đếm lại; trả về $1$ ngay khi đảo bị tách. Nếu không, cần xóa hai ô.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDays(self, grid: List[List[int]]) -> int:
        if self.count(grid) != 1:
            return 0
        m, n = len(grid), len(grid[0])
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    grid[i][j] = 0
                    if self.count(grid) != 1:
                        return 1
                    grid[i][j] = 1
        return 2

    def count(self, grid):
        def dfs(i, j):
            grid[i][j] = 2
            for a, b in [[0, -1], [0, 1], [1, 0], [-1, 0]]:
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and grid[x][y] == 1:
                    dfs(x, y)

        m, n = len(grid), len(grid[0])
        cnt = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    dfs(i, j)
                    cnt += 1
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 2:
                    grid[i][j] = 1
        return cnt
```

#### Java

```java
class Solution {
    private static final int[] DIRS = new int[] {-1, 0, 1, 0, -1};
    private int[][] grid;
    private int m;
    private int n;

    public int minDays(int[][] grid) {
        this.grid = grid;
        m = grid.length;
        n = grid[0].length;
        if (count() != 1) {
            return 0;
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    grid[i][j] = 0;
                    if (count() != 1) {
                        return 1;
                    }
                    grid[i][j] = 1;
                }
            }
        }
        return 2;
    }

    private int count() {
        int cnt = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    dfs(i, j);
                    ++cnt;
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 2) {
                    grid[i][j] = 1;
                }
            }
        }
        return cnt;
    }

    private void dfs(int i, int j) {
        grid[i][j] = 2;
        for (int k = 0; k < 4; ++k) {
            int x = i + DIRS[k], y = j + DIRS[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                dfs(x, y);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    const vector<int> dirs = {-1, 0, 1, 0, -1};
    int m, n;

    int minDays(vector<vector<int>>& grid) {
        m = grid.size(), n = grid[0].size();
        if (count(grid) != 1) {
            return 0;
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    grid[i][j] = 0;
                    if (count(grid) != 1) {
                        return 1;
                    }
                    grid[i][j] = 1;
                }
            }
        }
        return 2;
    }

    int count(vector<vector<int>>& grid) {
        int cnt = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    dfs(i, j, grid);
                    ++cnt;
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 2) {
                    grid[i][j] = 1;
                }
            }
        }
        return cnt;
    }

    void dfs(int i, int j, vector<vector<int>>& grid) {
        grid[i][j] = 2;
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                dfs(x, y, grid);
            }
        }
    }
};
```

#### Go

```go
func minDays(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	dirs := []int{-1, 0, 1, 0, -1}

	var dfs func(i, j int)
	dfs = func(i, j int) {
		grid[i][j] = 2
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1 {
				dfs(x, y)
			}
		}
	}

	count := func() int {
		cnt := 0
		for i := 0; i < m; i++ {
			for j := 0; j < n; j++ {
				if grid[i][j] == 1 {
					dfs(i, j)
					cnt++
				}
			}
		}
		for i := 0; i < m; i++ {
			for j := 0; j < n; j++ {
				if grid[i][j] == 2 {
					grid[i][j] = 1
				}
			}
		}
		return cnt
	}

	if count() != 1 {
		return 0
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 1 {
				grid[i][j] = 0
				if count() != 1 {
					return 1
				}
				grid[i][j] = 1
			}
		}
	}
	return 2
}
```

#### TypeScript

```ts
function minDays(grid: number[][]): number {
    const [m, n] = [grid.length, grid[0].length];

    const dfs = (i: number, j: number) => {
        if (i < 0 || m <= i || j < 0 || n <= j || [0, 2].includes(grid[i][j])) return;

        grid[i][j] = 2;
        const dir = [-1, 0, 1, 0, -1];
        for (let k = 0; k < 4; k++) {
            const [y, x] = [i + dir[k], j + dir[k + 1]];
            dfs(y, x);
        }
    };

    const count = () => {
        let c = 0;

        for (let i = 0; i < m; i++) {
            for (let j = 0; j < n; j++) {
                if (grid[i][j] === 1) {
                    dfs(i, j);
                    c++;
                }
            }
        }

        for (let i = 0; i < m; i++) {
            for (let j = 0; j < n; j++) {
                if (grid[i][j] === 2) {
                    grid[i][j] = 1;
                }
            }
        }

        return c;
    };

    if (count() !== 1) return 0;

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 1) {
                grid[i][j] = 0;

                if (count() !== 1) return 1;

                grid[i][j] = 1;
            }
        }
    }

    return 2;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var minDays = function (grid) {
    const dirs = [-1, 0, 1, 0, -1];
    const [m, n] = [grid.length, grid[0].length];

    const dfs = (i, j, visited) => {
        if (i < 0 || m <= i || j < 0 || n <= j || grid[i][j] === 0 || visited[i][j]) {
            return;
        }

        visited[i][j] = true;
        for (let d = 0; d < 4; d++) {
            const [y, x] = [i + dirs[d], j + dirs[d + 1]];
            dfs(y, x, visited);
        }
    };

    const count = () => {
        const vis = Array.from({ length: m }, () => Array(n).fill(false));
        let c = 0;
        for (let i = 0; i < m; i++) {
            for (let j = 0; j < n; j++) {
                if (grid[i][j] === 1 && !vis[i][j]) {
                    c++;
                    dfs(i, j, vis);
                }
            }
        }
        return c;
    };

    if (count() !== 1) return 0;

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 1) {
                grid[i][j] = 0;
                if (count() !== 1) return 1;
                grid[i][j] = 1;
            }
        }
    }

    return 2;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
