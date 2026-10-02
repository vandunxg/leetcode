---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [934. Shortest Bridge](https://leetcode.com/problems/shortest-bridge)

[中文文档](/solution/0900-0999/0934.Shortest%20Bridge/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>n x n</code> <code>grid</code>, trong đó <code>1</code> biểu thị đất liền và <code>0</code> biểu thị nước.</p>

<p><strong>Đảo</strong> là một nhóm các ô <code>1</code> liên thông theo bốn hướng và không nối với ô <code>1</code> nào khác. Trong <code>grid</code> có <strong>chính xác hai đảo</strong>.</p>

<p>Bạn có thể đổi các ô <code>0</code> thành <code>1</code> để nối hai đảo lại thành <strong>một đảo</strong>.</p>

<p>Hãy trả về <em>số ô </em><code>0</code><em> ít nhất cần đổi để nối hai đảo</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,1],[1,0]]
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,1,0],[0,0,0],[0,0,1]]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>grid[i][j]</code> bằng <code>0</code> hoặc <code>1</code>.</li>
	<li><code>grid</code> có chính xác hai đảo.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có chính xác hai đảo, nên số lần đổi ít nhất bằng khoảng cách ngắn nhất giữa chúng. Dùng DFS để đưa toàn bộ ô của một đảo vào queue và đánh dấu đã thăm, sau đó mở rộng BFS qua các ô nước. Lớp đầu tiên chạm đến đảo còn lại chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestBridge(self, grid: List[List[int]]) -> int:
        def dfs(i, j):
            q.append((i, j))
            grid[i][j] = 2
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < n and 0 <= y < n and grid[x][y] == 1:
                    dfs(x, y)

        n = len(grid)
        dirs = (-1, 0, 1, 0, -1)
        q = deque()
        i, j = next((i, j) for i in range(n) for j in range(n) if grid[i][j])
        dfs(i, j)
        ans = 0
        while 1:
            for _ in range(len(q)):
                i, j = q.popleft()
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    if 0 <= x < n and 0 <= y < n:
                        if grid[x][y] == 1:
                            return ans
                        if grid[x][y] == 0:
                            grid[x][y] = 2
                            q.append((x, y))
            ans += 1
```

#### Java

```java
class Solution {
    private int[] dirs = {-1, 0, 1, 0, -1};
    private Deque<int[]> q = new ArrayDeque<>();
    private int[][] grid;
    private int n;

    public int shortestBridge(int[][] grid) {
        this.grid = grid;
        n = grid.length;
        for (int i = 0, x = 1; i < n && x == 1; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    dfs(i, j);
                    x = 0;
                    break;
                }
            }
        }
        int ans = 0;
        while (true) {
            for (int i = q.size(); i > 0; --i) {
                var p = q.pollFirst();
                for (int k = 0; k < 4; ++k) {
                    int x = p[0] + dirs[k], y = p[1] + dirs[k + 1];
                    if (x >= 0 && x < n && y >= 0 && y < n) {
                        if (grid[x][y] == 1) {
                            return ans;
                        }
                        if (grid[x][y] == 0) {
                            grid[x][y] = 2;
                            q.offer(new int[] {x, y});
                        }
                    }
                }
            }
            ++ans;
        }
    }

    private void dfs(int i, int j) {
        grid[i][j] = 2;
        q.offer(new int[] {i, j});
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] == 1) {
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
    const static inline vector<int> dirs = {-1, 0, 1, 0, -1};

    int shortestBridge(vector<vector<int>>& grid) {
        int n = grid.size();
        queue<pair<int, int>> q;
        function<void(int, int)> dfs = [&](int i, int j) {
            grid[i][j] = 2;
            q.emplace(i, j);
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] == 1) {
                    dfs(x, y);
                }
            }
        };
        for (int i = 0, x = 1; i < n && x; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    dfs(i, j);
                    x = 0;
                    break;
                }
            }
        }
        int ans = 0;
        while (1) {
            for (int h = q.size(); h; --h) {
                auto [i, j] = q.front();
                q.pop();
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    if (x >= 0 && x < n && y >= 0 && y < n) {
                        if (grid[x][y] == 1) return ans;
                        if (grid[x][y] == 0) {
                            grid[x][y] = 2;
                            q.emplace(x, y);
                        }
                    }
                }
            }
            ++ans;
        }
    }
};
```

#### Go

```go
func shortestBridge(grid [][]int) (ans int) {
	n := len(grid)
	dirs := []int{-1, 0, 1, 0, -1}
	type pair struct{ i, j int }
	q := []pair{}
	var dfs func(int, int)
	dfs = func(i, j int) {
		grid[i][j] = 2
		q = append(q, pair{i, j})
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < n && y >= 0 && y < n && grid[x][y] == 1 {
				dfs(x, y)
			}
		}
	}
	for i, x := 0, 1; i < n && x == 1; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 1 {
				dfs(i, j)
				x = 0
				break
			}
		}
	}
	for {
		for i := len(q); i > 0; i-- {
			p := q[0]
			q = q[1:]
			for k := 0; k < 4; k++ {
				x, y := p.i+dirs[k], p.j+dirs[k+1]
				if x >= 0 && x < n && y >= 0 && y < n {
					if grid[x][y] == 1 {
						return
					}
					if grid[x][y] == 0 {
						grid[x][y] = 2
						q = append(q, pair{x, y})
					}
				}
			}
		}
		ans++
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
