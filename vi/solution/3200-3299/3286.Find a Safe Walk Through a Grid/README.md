---
comments: true
difficulty: Medium
rating: 1607
source: Biweekly Contest 139 Q2
tags:
    - Breadth-First Search
    - Graph
    - Array
    - Matrix
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3286. Find a Safe Walk Through a Grid](https://leetcode.com/problems/find-a-safe-walk-through-a-grid)

[中文文档](/solution/3200-3299/3286.Find%20a%20Safe%20Walk%20Through%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận nhị phân <code>m x n</code> là <code>grid</code> và một số nguyên <code>health</code>.</p>

<p>Bạn bắt đầu ở góc trên bên trái <code>(0, 0)</code> và muốn đi đến góc dưới bên phải <code>(m - 1, n - 1)</code>.</p>

<p>Bạn có thể di chuyển lên, xuống, trái hoặc phải từ một ô đến một ô kề nhau, miễn là health của bạn <em>vẫn còn</em> <strong>dương</strong>.</p>

<p>Các ô <code>(i, j)</code> có <code>grid[i][j] = 1</code> được xem là <strong>không an toàn</strong> và làm giảm health của bạn đi 1.</p>

<p>Trả về <code>true</code> nếu bạn có thể đến ô cuối cùng với giá trị health lớn hơn hoặc bằng 1, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]], health = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể đến ô cuối cùng một cách an toàn bằng cách đi qua các ô màu xám bên dưới.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3286.Find%20a%20Safe%20Walk%20Through%20a%20Grid/images/3868_examples_1drawio.png" style="width: 301px; height: 121px;" /></div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,1,1,0,0,0],[1,0,1,0,0,0],[0,1,1,1,0,1],[0,0,1,0,1,0]], health = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cần ít nhất 4 điểm health để đến ô cuối cùng một cách an toàn.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3286.Find%20a%20Safe%20Walk%20Through%20a%20Grid/images/3868_examples_2drawio.png" style="width: 361px; height: 161px;" /></div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,1,1],[1,0,1],[1,1,1]], health = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể đến ô cuối cùng một cách an toàn bằng cách đi qua các ô màu xám bên dưới.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3286.Find%20a%20Safe%20Walk%20Through%20a%20Grid/images/3868_examples_3drawio.png" style="width: 181px; height: 121px;" /></p>

<p>Mọi đường đi không đi qua ô <code>(1, 1)</code> đều không an toàn vì health của bạn sẽ giảm xuống 0 khi đến ô cuối cùng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code><font face="monospace">2 &lt;= m * n</font></code></li>
	<li><code>1 &lt;= health &lt;= m + n</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là 0 hoặc 1.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ô có giá trị $1$ tiêu tốn một đơn vị health; ta phải đến đích mà vẫn còn health. Vì $m,n\le 50$, BFS chỉ đánh dấu ô đã thăm là chưa đủ, bởi một đường đi ít tốn kém hơn có thể đến sau.
>
> $dist[i][j]$ là chi phí nhỏ nhất để đến ô đó; mỗi lần relaxation thành công, ta đưa ô vào queue. Trọng số chỉ là $0/1$, nên BFS dùng queue là hợp lệ. Có thể đến đích an toàn khi và chỉ khi chi phí đó nhỏ hơn nghiêm ngặt $\textit{health}$.

<!-- thinking:end -->

Ta định nghĩa một mảng 2D $\textit{dist}$, trong đó $\textit{dist}[i][j]$ biểu thị giá trị health tối thiểu cần có để đi từ góc trên bên trái đến vị trí $(i, j)$. Ban đầu, ta đặt $\textit{dist}[0][0]$ bằng $\textit{grid}[0][0]$ và thêm $(0, 0)$ vào queue $\textit{q}$.

Sau đó, ta liên tục lấy các phần tử $(x, y)$ khỏi queue và thử di chuyển theo bốn hướng. Nếu ta di chuyển đến vị trí hợp lệ $(nx, ny)$ và giá trị health cần để đi từ $(x, y)$ đến $(nx, ny)$ nhỏ hơn giá trị hiện tại, ta cập nhật $\textit{dist}[nx][ny] = \textit{dist}[x][y] + \textit{grid}[nx][ny]$ và thêm $(nx, ny)$ vào queue $\textit{q}$.

Cuối cùng, khi queue rỗng, ta nhận được $\textit{dist}[m-1][n-1]$, là giá trị health tối thiểu cần có để đi từ góc trên bên trái đến góc dưới bên phải. Nếu giá trị này nhỏ hơn $\textit{health}$, ta có thể đến góc dưới bên phải; nếu không thì không thể.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSafeWalk(self, grid: List[List[int]], health: int) -> bool:
        m, n = len(grid), len(grid[0])
        dist = [[inf] * n for _ in range(m)]
        dist[0][0] = grid[0][0]
        q = deque([(0, 0)])
        dirs = (-1, 0, 1, 0, -1)
        while q:
            x, y = q.popleft()
            for a, b in pairwise(dirs):
                nx, ny = x + a, y + b
                if (
                    0 <= nx < m
                    and 0 <= ny < n
                    and dist[nx][ny] > dist[x][y] + grid[nx][ny]
                ):
                    dist[nx][ny] = dist[x][y] + grid[nx][ny]
                    q.append((nx, ny))
        return dist[-1][-1] < health
```

#### Java

```java
class Solution {
    public boolean findSafeWalk(List<List<Integer>> grid, int health) {
        int m = grid.size();
        int n = grid.get(0).size();
        int[][] dist = new int[m][n];
        for (int[] row : dist) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        dist[0][0] = grid.get(0).get(0);
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {0, 0});
        final int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            int[] curr = q.poll();
            int x = curr[0], y = curr[1];
            for (int i = 0; i < 4; i++) {
                int nx = x + dirs[i];
                int ny = y + dirs[i + 1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n
                    && dist[nx][ny] > dist[x][y] + grid.get(nx).get(ny)) {
                    dist[nx][ny] = dist[x][y] + grid.get(nx).get(ny);
                    q.offer(new int[] {nx, ny});
                }
            }
        }
        return dist[m - 1][n - 1] < health;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool findSafeWalk(vector<vector<int>>& grid, int health) {
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
        dist[0][0] = grid[0][0];
        queue<pair<int, int>> q;
        q.emplace(0, 0);
        int dirs[5] = {-1, 0, 1, 0, -1};
        while (!q.empty()) {
            auto [x, y] = q.front();
            q.pop();
            for (int i = 0; i < 4; ++i) {
                int nx = x + dirs[i];
                int ny = y + dirs[i + 1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && dist[nx][ny] > dist[x][y] + grid[nx][ny]) {
                    dist[nx][ny] = dist[x][y] + grid[nx][ny];
                    q.emplace(nx, ny);
                }
            }
        }
        return dist[m - 1][n - 1] < health;
    }
};
```

#### Go

```go
func findSafeWalk(grid [][]int, health int) bool {
	m, n := len(grid), len(grid[0])
	dist := make([][]int, m)
	for i := range dist {
		dist[i] = make([]int, n)
		for j := range dist[i] {
			dist[i][j] = math.MaxInt32
		}
	}
	dist[0][0] = grid[0][0]
	q := [][2]int{{0, 0}}
	dirs := []int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		curr := q[0]
		q = q[1:]
		x, y := curr[0], curr[1]
		for i := 0; i < 4; i++ {
			nx, ny := x+dirs[i], y+dirs[i+1]
			if nx >= 0 && nx < m && ny >= 0 && ny < n && dist[nx][ny] > dist[x][y]+grid[nx][ny] {
				dist[nx][ny] = dist[x][y] + grid[nx][ny]
				q = append(q, [2]int{nx, ny})
			}
		}
	}
	return dist[m-1][n-1] < health
}
```

#### TypeScript

```ts
function findSafeWalk(grid: number[][], health: number): boolean {
    const m = grid.length;
    const n = grid[0].length;
    const dist: number[][] = Array.from({ length: m }, () => Array(n).fill(Infinity));
    dist[0][0] = grid[0][0];
    const q: [number, number][] = [[0, 0]];
    const dirs = [-1, 0, 1, 0, -1];
    while (q.length > 0) {
        const [x, y] = q.shift()!;
        for (let i = 0; i < 4; i++) {
            const nx = x + dirs[i];
            const ny = y + dirs[i + 1];
            if (
                nx >= 0 &&
                nx < m &&
                ny >= 0 &&
                ny < n &&
                dist[nx][ny] > dist[x][y] + grid[nx][ny]
            ) {
                dist[nx][ny] = dist[x][y] + grid[nx][ny];
                q.push([nx, ny]);
            }
        }
    }
    return dist[m - 1][n - 1] < health;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_safe_walk(grid: Vec<Vec<i32>>, health: i32) -> bool {
        let m = grid.len();
        let n = grid[0].len();
        let inf: i32 = i32::MAX / 2;

        let mut dist = vec![vec![inf; n]; m];
        dist[0][0] = grid[0][0];

        let mut q = std::collections::VecDeque::new();
        q.push_back((0usize, 0usize));

        let dirs = [(-1i32, 0i32), (1, 0), (0, -1), (0, 1)];

        while let Some((x, y)) = q.pop_front() {
            for (dx, dy) in dirs {
                let nx = x as i32 + dx;
                let ny = y as i32 + dy;

                if nx >= 0 && nx < m as i32 && ny >= 0 && ny < n as i32 {
                    let nx = nx as usize;
                    let ny = ny as usize;

                    let new_cost = dist[x][y] + grid[nx][ny];
                    if new_cost < dist[nx][ny] {
                        dist[nx][ny] = new_cost;
                        q.push_back((nx, ny));
                    }
                }
            }
        }

        dist[m - 1][n - 1] < health
    }
}
```

#### C#

```cs
public class Solution {
    public bool FindSafeWalk(IList<IList<int>> grid, int health) {
        int m = grid.Count;
        int n = grid[0].Count;
        int inf = int.MaxValue / 2;

        int[,] dist = new int[m, n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                dist[i, j] = inf;
            }
        }

        dist[0, 0] = grid[0][0];

        var q = new Queue<(int x, int y)>();
        q.Enqueue((0, 0));

        int[] dirs = new int[] { -1, 0, 1, 0, -1 };

        while (q.Count > 0) {
            var (x, y) = q.Dequeue();

            for (int i = 0; i < 4; i++) {
                int nx = x + dirs[i];
                int ny = y + dirs[i + 1];

                if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                    int newCost = dist[x, y] + grid[nx][ny];
                    if (newCost < dist[nx, ny]) {
                        dist[nx, ny] = newCost;
                        q.Enqueue((nx, ny));
                    }
                }
            }
        }

        return dist[m - 1, n - 1] < health;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
