---
comments: true
difficulty: Hard
rating: 1967
source: Weekly Contest 167 Q4
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [1293. Shortest Path in a Grid with Obstacles Elimination](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination)

[中文文档](/solution/1200-1299/1293.Shortest%20Path%20in%20a%20Grid%20with%20Obstacles%20Elimination/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên <code>m x n</code> <code>grid</code>, trong đó mỗi ô là <code>0</code> (ô trống) hoặc <code>1</code> (chướng ngại vật). Trong <strong>một bước</strong>, bạn có thể di chuyển lên, xuống, trái hoặc phải giữa các ô trống.</p>

<p>Trả về <em>số <strong>bước</strong> ít nhất để đi từ góc trên bên trái </em><code>(0, 0)</code><em> đến góc dưới bên phải </em><code>(m - 1, n - 1)</code><em>, với điều kiện bạn có thể loại bỏ <strong>nhiều nhất</strong> </em><code>k</code><em> chướng ngại vật</em>. Nếu không thể tìm được đường đi như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1293.Shortest%20Path%20in%20a%20Grid%20with%20Obstacles%20Elimination/images/short1-grid.jpg" style="width: 244px; height: 405px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,0,0],[1,1,0],[0,0,0],[0,1,1],[0,0,0]], k = 1
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> 
Đường đi ngắn nhất nếu không loại bỏ chướng ngại vật nào dài 10 bước.
Đường đi ngắn nhất khi loại bỏ một chướng ngại vật tại vị trí (3,2) dài 6 bước. Đường đi đó là (0,0) -&gt; (0,1) -&gt; (0,2) -&gt; (1,2) -&gt; (2,2) -&gt; <strong>(3,2)</strong> -&gt; (4,2).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1293.Shortest%20Path%20in%20a%20Grid%20with%20Obstacles%20Elimination/images/short2-grid.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,1],[1,1,1],[1,0,0]], k = 1
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Cần loại bỏ ít nhất hai chướng ngại vật mới tìm được đường đi như vậy.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 40</code></li>
	<li><code>1 &lt;= k &lt;= m * n</code></li>
	<li><code>grid[i][j]</code> là <code>0</code> <strong>hoặc</strong> <code>1</code>.</li>
	<li><code>grid[0][0] == grid[m - 1][n - 1] == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm đường đi ngắn nhất trên grid và được phép loại bỏ tối đa $k$ chướng ngại vật. Vì $m,n \le 40$, state cần bao gồm số lần loại bỏ còn lại. Mỗi layer của BFS tương ứng với số bước; nếu $k$ đủ để loại bỏ chướng ngại vật trên đường Manhattan thì trả về độ dài đường đi đó.
>
> Queue lưu $(i,j,k\text{ left})$. Khi đi vào ô trống, giữ nguyên $k$; khi đi vào chướng ngại vật, giảm $k$ đi $1$. Cùng một ô nhưng có số lần loại bỏ còn lại khác nhau sẽ là các state khác nhau. Lần đầu đến đích chính là đường đi ngắn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestPath(self, grid: List[List[int]], k: int) -> int:
        m, n = len(grid), len(grid[0])
        if k >= m + n - 3:
            return m + n - 2
        q = deque([(0, 0, k)])
        vis = {(0, 0, k)}
        ans = 0
        while q:
            ans += 1
            for _ in range(len(q)):
                i, j, k = q.popleft()
                for a, b in [[0, -1], [0, 1], [1, 0], [-1, 0]]:
                    x, y = i + a, j + b
                    if 0 <= x < m and 0 <= y < n:
                        if x == m - 1 and y == n - 1:
                            return ans
                        if grid[x][y] == 0 and (x, y, k) not in vis:
                            q.append((x, y, k))
                            vis.add((x, y, k))
                        if grid[x][y] == 1 and k > 0 and (x, y, k - 1) not in vis:
                            q.append((x, y, k - 1))
                            vis.add((x, y, k - 1))
        return -1
```

#### Java

```java
class Solution {
    public int shortestPath(int[][] grid, int k) {
        int m = grid.length;
        int n = grid[0].length;
        if (k >= m + n - 3) {
            return m + n - 2;
        }
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {0, 0, k});
        boolean[][][] vis = new boolean[m][n][k + 1];
        vis[0][0][k] = true;
        int ans = 0;
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            ++ans;
            for (int i = q.size(); i > 0; --i) {
                int[] p = q.poll();
                k = p[2];
                for (int j = 0; j < 4; ++j) {
                    int x = p[0] + dirs[j];
                    int y = p[1] + dirs[j + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n) {
                        if (x == m - 1 && y == n - 1) {
                            return ans;
                        }
                        if (grid[x][y] == 0 && !vis[x][y][k]) {
                            q.offer(new int[] {x, y, k});
                            vis[x][y][k] = true;
                        } else if (grid[x][y] == 1 && k > 0 && !vis[x][y][k - 1]) {
                            q.offer(new int[] {x, y, k - 1});
                            vis[x][y][k - 1] = true;
                        }
                    }
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shortestPath(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        if (k >= m + n - 3) return m + n - 2;
        queue<vector<int>> q;
        q.push({0, 0, k});
        vector<vector<vector<bool>>> vis(m, vector<vector<bool>>(n, vector<bool>(k + 1)));
        vis[0][0][k] = true;
        int ans = 0;
        vector<int> dirs = {-1, 0, 1, 0, -1};
        while (!q.empty()) {
            ++ans;
            for (int i = q.size(); i > 0; --i) {
                auto p = q.front();
                k = p[2];
                q.pop();
                for (int j = 0; j < 4; ++j) {
                    int x = p[0] + dirs[j], y = p[1] + dirs[j + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n) {
                        if (x == m - 1 && y == n - 1) return ans;
                        if (grid[x][y] == 0 && !vis[x][y][k]) {
                            q.push({x, y, k});
                            vis[x][y][k] = true;
                        } else if (grid[x][y] == 1 && k > 0 && !vis[x][y][k - 1]) {
                            q.push({x, y, k - 1});
                            vis[x][y][k - 1] = true;
                        }
                    }
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func shortestPath(grid [][]int, k int) int {
	m, n := len(grid), len(grid[0])
	if k >= m+n-3 {
		return m + n - 2
	}
	q := [][]int{[]int{0, 0, k}}
	vis := make([][][]bool, m)
	for i := range vis {
		vis[i] = make([][]bool, n)
		for j := range vis[i] {
			vis[i][j] = make([]bool, k+1)
		}
	}
	vis[0][0][k] = true
	dirs := []int{-1, 0, 1, 0, -1}
	ans := 0
	for len(q) > 0 {
		ans++
		for i := len(q); i > 0; i-- {
			p := q[0]
			q = q[1:]
			k = p[2]
			for j := 0; j < 4; j++ {
				x, y := p[0]+dirs[j], p[1]+dirs[j+1]
				if x >= 0 && x < m && y >= 0 && y < n {
					if x == m-1 && y == n-1 {
						return ans
					}
					if grid[x][y] == 0 && !vis[x][y][k] {
						q = append(q, []int{x, y, k})
						vis[x][y][k] = true
					} else if grid[x][y] == 1 && k > 0 && !vis[x][y][k-1] {
						q = append(q, []int{x, y, k - 1})
						vis[x][y][k-1] = true
					}
				}
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function shortestPath(grid: number[][], k: number): number {
    const m = grid.length;
    const n = grid[0].length;
    if (k >= m + n - 3) {
        return m + n - 2;
    }

    let q: Point[] = [[0, 0, k]];
    const vis = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array.from({ length: k + 1 }, () => false)),
    );
    vis[0][0][k] = true;
    const dirs = [0, 1, 0, -1, 0];
    let ans = 0;

    while (q.length) {
        const nextQ: Point[] = [];
        ++ans;

        for (const [i, j, k] of q) {
            for (let d = 0; d < 4; ++d) {
                const [x, y] = [i + dirs[d], j + dirs[d + 1]];
                if (x === m - 1 && y === n - 1) {
                    return ans;
                }
                const v = grid[x]?.[y];
                if (v === 0 && !vis[x][y][k]) {
                    nextQ.push([x, y, k]);
                    vis[x][y][k] = true;
                } else if (v === 1 && k > 0 && !vis[x][y][k - 1]) {
                    nextQ.push([x, y, k - 1]);
                    vis[x][y][k - 1] = true;
                }
            }
        }
        q = nextQ;
    }
    return -1;
}

type Point = [number, number, number];
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
