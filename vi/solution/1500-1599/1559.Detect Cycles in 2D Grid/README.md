---
comments: true
difficulty: Medium
rating: 1837
source: Biweekly Contest 33 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Matrix
---

<!-- problem:start -->

# [1559. Detect Cycles in 2D Grid](https://leetcode.com/problems/detect-cycles-in-2d-grid)

[中文文档](/solution/1500-1599/1559.Detect%20Cycles%20in%202D%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng ký tự 2D <code>grid</code> kích thước <code>m x n</code>, hãy xác định xem trong <code>grid</code> có chu kỳ nào gồm các ô có <strong>cùng giá trị</strong> hay không.</p>

<p>Chu kỳ là một đường đi có <strong>độ dài từ 4 trở lên</strong> trong lưới, bắt đầu và kết thúc tại cùng một ô. Từ một ô, bạn có thể đi tới ô kề nó theo một trong bốn hướng (lên, xuống, trái hoặc phải) nếu ô đó có <strong>cùng giá trị</strong> với ô hiện tại.</p>

<p>Ngoài ra, không được đi tới ô đã ghé thăm trong bước ngay trước đó. Ví dụ, chu kỳ <code>(1, 1) -&gt; (1, 2) -&gt; (1, 1)</code> không hợp lệ vì từ <code>(1, 2)</code> ta quay lại <code>(1, 1)</code>, là ô vừa ghé thăm.</p>

<p>Trả về <code>true</code> nếu tồn tại chu kỳ gồm các ô cùng giá trị trong <code>grid</code>, nếu không thì trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1559.Detect%20Cycles%20in%202D%20Grid/images/1.png" style="width: 231px; height: 152px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[&quot;a&quot;,&quot;a&quot;,&quot;a&quot;,&quot;a&quot;],[&quot;a&quot;,&quot;b&quot;,&quot;b&quot;,&quot;a&quot;],[&quot;a&quot;,&quot;b&quot;,&quot;b&quot;,&quot;a&quot;],[&quot;a&quot;,&quot;a&quot;,&quot;a&quot;,&quot;a&quot;]]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Có hai chu kỳ hợp lệ được hiển thị bằng các màu khác nhau trong hình dưới đây:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1559.Detect%20Cycles%20in%202D%20Grid/images/11.png" style="width: 225px; height: 163px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1559.Detect%20Cycles%20in%202D%20Grid/images/22.png" style="width: 236px; height: 154px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[&quot;c&quot;,&quot;c&quot;,&quot;c&quot;,&quot;a&quot;],[&quot;c&quot;,&quot;d&quot;,&quot;c&quot;,&quot;c&quot;],[&quot;c&quot;,&quot;c&quot;,&quot;e&quot;,&quot;c&quot;],[&quot;f&quot;,&quot;c&quot;,&quot;c&quot;,&quot;c&quot;]]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Chỉ có một chu kỳ hợp lệ được đánh dấu trong hình dưới đây:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1559.Detect%20Cycles%20in%202D%20Grid/images/2.png" style="width: 229px; height: 157px;" />
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1559.Detect%20Cycles%20in%202D%20Grid/images/3.png" style="width: 183px; height: 120px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[&quot;a&quot;,&quot;b&quot;,&quot;b&quot;],[&quot;b&quot;,&quot;z&quot;,&quot;b&quot;],[&quot;b&quot;,&quot;b&quot;,&quot;a&quot;]]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>grid</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Phát hiện chu kỳ gồm các chữ cái giống nhau có độ dài ít nhất $4$. Lưới đủ nhỏ để duyệt đồ thị một lần, nhưng đi thẳng ngược về node cha không được tính là chu kỳ.
>
> Từ mỗi ô chưa thăm, chạy BFS, chỉ đi tới các ô kề có cùng chữ cái và ghi nhớ node cha. Một ô kề đã được thăm nhưng không phải node cha sẽ khép lại một chu kỳ. Mỗi thành phần liên thông chỉ được duyệt một lần.

<!-- thinking:end -->

Ta có thể duyệt từng ô trong lưới 2D. Với mỗi ô, nếu ô $grid[i][j]$ chưa được thăm, ta bắt đầu BFS từ ô đó. Trong quá trình duyệt, cần ghi lại node cha của mỗi ô và tọa độ của ô trước đó. Nếu giá trị của ô kế tiếp giống ô hiện tại, không phải ô trước đó và đã được thăm, thì đã tìm thấy chu kỳ và ta trả về $\textit{true}$. Sau khi duyệt tất cả các ô, nếu không tìm thấy chu kỳ thì trả về $\textit{false}$.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của lưới 2D.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def containsCycle(self, grid: List[List[str]]) -> bool:
        m, n = len(grid), len(grid[0])
        vis = [[False] * n for _ in range(m)]
        dirs = (-1, 0, 1, 0, -1)
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if vis[i][j]:
                    continue
                vis[i][j] = True
                q = [(i, j, -1, -1)]
                while q:
                    x, y, px, py = q.pop()
                    for dx, dy in pairwise(dirs):
                        nx, ny = x + dx, y + dy
                        if 0 <= nx < m and 0 <= ny < n:
                            if grid[nx][ny] != grid[i][j] or (nx == px and ny == py):
                                continue
                            if vis[nx][ny]:
                                return True
                            vis[nx][ny] = True
                            q.append((nx, ny, x, y))
        return False
```

#### Java

```java
class Solution {
    public boolean containsCycle(char[][] grid) {
        int m = grid.length, n = grid[0].length;
        boolean[][] vis = new boolean[m][n];
        final int[] dirs = {-1, 0, 1, 0, -1};
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (!vis[i][j]) {
                    Deque<int[]> q = new ArrayDeque<>();
                    q.offer(new int[] {i, j, -1, -1});
                    vis[i][j] = true;
                    while (!q.isEmpty()) {
                        int[] p = q.poll();
                        int x = p[0], y = p[1], px = p[2], py = p[3];
                        for (int k = 0; k < 4; ++k) {
                            int nx = x + dirs[k], ny = y + dirs[k + 1];
                            if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                                if (grid[nx][ny] != grid[x][y] || (nx == px && ny == py)) {
                                    continue;
                                }
                                if (vis[nx][ny]) {
                                    return true;
                                }
                                q.offer(new int[] {nx, ny, x, y});
                                vis[nx][ny] = true;
                            }
                        }
                    }
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool containsCycle(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<bool>> vis(m, vector<bool>(n));
        const vector<int> dirs = {-1, 0, 1, 0, -1};

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (!vis[i][j]) {
                    queue<array<int, 4>> q;
                    q.push({i, j, -1, -1});
                    vis[i][j] = true;

                    while (!q.empty()) {
                        auto p = q.front();
                        q.pop();
                        int x = p[0], y = p[1], px = p[2], py = p[3];

                        for (int k = 0; k < 4; ++k) {
                            int nx = x + dirs[k], ny = y + dirs[k + 1];
                            if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                                if (grid[nx][ny] != grid[x][y] || (nx == px && ny == py)) {
                                    continue;
                                }
                                if (vis[nx][ny]) {
                                    return true;
                                }
                                q.push({nx, ny, x, y});
                                vis[nx][ny] = true;
                            }
                        }
                    }
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func containsCycle(grid [][]byte) bool {
	m, n := len(grid), len(grid[0])
	vis := make([][]bool, m)
	for i := range vis {
		vis[i] = make([]bool, n)
	}
	dirs := []int{-1, 0, 1, 0, -1}

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if !vis[i][j] {
				q := [][]int{{i, j, -1, -1}}
				vis[i][j] = true

				for len(q) > 0 {
					p := q[0]
					q = q[1:]
					x, y, px, py := p[0], p[1], p[2], p[3]

					for k := 0; k < 4; k++ {
						nx, ny := x+dirs[k], y+dirs[k+1]
						if nx >= 0 && nx < m && ny >= 0 && ny < n {
							if grid[nx][ny] != grid[x][y] || (nx == px && ny == py) {
								continue
							}
							if vis[nx][ny] {
								return true
							}
							q = append(q, []int{nx, ny, x, y})
							vis[nx][ny] = true
						}
					}
				}
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function containsCycle(grid: string[][]): boolean {
    const [m, n] = [grid.length, grid[0].length];
    const vis: boolean[][] = Array.from({ length: m }, () => Array(n).fill(false));
    const dirs = [-1, 0, 1, 0, -1];
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (!vis[i][j]) {
                const q: [number, number, number, number][] = [[i, j, -1, -1]];
                vis[i][j] = true;
                for (const [x, y, px, py] of q) {
                    for (let k = 0; k < 4; k++) {
                        const [nx, ny] = [x + dirs[k], y + dirs[k + 1]];
                        if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                            if (grid[nx][ny] !== grid[x][y] || (nx === px && ny === py)) {
                                continue;
                            }
                            if (vis[nx][ny]) {
                                return true;
                            }
                            q.push([nx, ny, x, y]);
                            vis[nx][ny] = true;
                        }
                    }
                }
            }
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn contains_cycle(grid: Vec<Vec<char>>) -> bool {
        let m = grid.len();
        let n = grid[0].len();
        let mut vis = vec![vec![false; n]; m];
        let dirs = vec![-1, 0, 1, 0, -1];

        for i in 0..m {
            for j in 0..n {
                if !vis[i][j] {
                    let mut q = vec![(i as isize, j as isize, -1, -1)];
                    vis[i][j] = true;

                    while !q.is_empty() {
                        let (x, y, px, py) = q.pop().unwrap();

                        for k in 0..4 {
                            let nx = x + dirs[k];
                            let ny = y + dirs[k + 1];
                            if nx >= 0 && nx < m as isize && ny >= 0 && ny < n as isize {
                                let nx = nx as usize;
                                let ny = ny as usize;
                                if grid[nx][ny] != grid[x as usize][y as usize]
                                    || (nx == px as usize && ny == py as usize)
                                {
                                    continue;
                                }
                                if vis[nx][ny] {
                                    return true;
                                }
                                q.push((nx as isize, ny as isize, x, y));
                                vis[nx][ny] = true;
                            }
                        }
                    }
                }
            }
        }
        false
    }
}
```

#### JavaScript

```js
/**
 * @param {character[][]} grid
 * @return {boolean}
 */
var containsCycle = function (grid) {
    const [m, n] = [grid.length, grid[0].length];
    const vis = Array.from({ length: m }, () => Array(n).fill(false));
    const dirs = [-1, 0, 1, 0, -1];
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (!vis[i][j]) {
                const q = [[i, j, -1, -1]];
                vis[i][j] = true;
                for (const [x, y, px, py] of q) {
                    for (let k = 0; k < 4; k++) {
                        const [nx, ny] = [x + dirs[k], y + dirs[k + 1]];
                        if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                            if (grid[nx][ny] !== grid[x][y] || (nx === px && ny === py)) {
                                continue;
                            }
                            if (vis[nx][ny]) {
                                return true;
                            }
                            q.push([nx, ny, x, y]);
                            vis[nx][ny] = true;
                        }
                    }
                }
            }
        }
    }
    return false;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: DFS

<!-- thinking:start -->

> **Tư duy**
>
> BFS cần một queue tường minh. DFS dùng call stack cho cùng phép kiểm tra node cha và có phần cài đặt ngắn hơn. Cách đánh dấu đã thăm và quy tắc “ô đã thăm không phải cha nghĩa là có chu kỳ” vẫn giữ nguyên; chỉ thứ tự duyệt thay đổi.

<!-- thinking:end -->

Ta có thể duyệt từng ô trong lưới 2D. Với mỗi ô, nếu ô $grid[i][j]$ chưa được thăm, ta bắt đầu DFS từ ô đó. Trong quá trình duyệt, cần ghi lại node cha của mỗi ô và tọa độ của ô trước đó. Nếu giá trị của ô kế tiếp giống ô hiện tại, không phải ô trước đó và đã được thăm, thì đã tìm thấy chu kỳ và ta trả về $\textit{true}$. Sau khi duyệt tất cả các ô, nếu không tìm thấy chu kỳ thì trả về $\textit{false}$.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của lưới 2D.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def containsCycle(self, grid: List[List[str]]) -> bool:
        def dfs(x: int, y: int, px: int, py: int) -> bool:
            vis[x][y] = True
            for dx, dy in pairwise(dirs):
                nx, ny = x + dx, y + dy
                if 0 <= nx < m and 0 <= ny < n:
                    if grid[nx][ny] != grid[x][y] or (nx == px and ny == py):
                        continue
                    if vis[nx][ny] or dfs(nx, ny, x, y):
                        return True
            return False

        m, n = len(grid), len(grid[0])
        vis = [[False] * n for _ in range(m)]
        dirs = (-1, 0, 1, 0, -1)
        for i in range(m):
            for j in range(n):
                if vis[i][j]:
                    continue
                if dfs(i, j, -1, -1):
                    return True
        return False
```

#### Java

```java
class Solution {
    private char[][] grid;
    private boolean[][] vis;
    private final int[] dirs = {-1, 0, 1, 0, -1};
    private int m;
    private int n;

    public boolean containsCycle(char[][] grid) {
        this.grid = grid;
        m = grid.length;
        n = grid[0].length;
        vis = new boolean[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (!vis[i][j] && dfs(i, j, -1, -1)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean dfs(int x, int y, int px, int py) {
        vis[x][y] = true;
        for (int k = 0; k < 4; ++k) {
            int nx = x + dirs[k], ny = y + dirs[k + 1];
            if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                if (grid[nx][ny] != grid[x][y] || (nx == px && ny == py)) {
                    continue;
                }
                if (vis[nx][ny] || dfs(nx, ny, x, y)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool containsCycle(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<bool>> vis(m, vector<bool>(n));
        const vector<int> dirs = {-1, 0, 1, 0, -1};
        auto dfs = [&](this auto&& dfs, int x, int y, int px, int py) -> bool {
            vis[x][y] = true;
            for (int k = 0; k < 4; ++k) {
                int nx = x + dirs[k], ny = y + dirs[k + 1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                    if (grid[nx][ny] != grid[x][y] || (nx == px && ny == py)) {
                        continue;
                    }
                    if (vis[nx][ny] || dfs(nx, ny, x, y)) {
                        return true;
                    }
                }
            }
            return false;
        };
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (!vis[i][j] && dfs(i, j, -1, -1)) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func containsCycle(grid [][]byte) bool {
	m, n := len(grid), len(grid[0])
	vis := make([][]bool, m)
	for i := range vis {
		vis[i] = make([]bool, n)
	}
	dirs := []int{-1, 0, 1, 0, -1}
	var dfs func(x, y, px, py int) bool
	dfs = func(x, y, px, py int) bool {
		vis[x][y] = true
		for k := 0; k < 4; k++ {
			nx, ny := x+dirs[k], y+dirs[k+1]
			if nx >= 0 && nx < m && ny >= 0 && ny < n {
				if grid[nx][ny] != grid[x][y] || (nx == px && ny == py) {
					continue
				}
				if vis[nx][ny] || dfs(nx, ny, x, y) {
					return true
				}
			}
		}
		return false
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if !vis[i][j] && dfs(i, j, -1, -1) {
				return true
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function containsCycle(grid: string[][]): boolean {
    const [m, n] = [grid.length, grid[0].length];
    const vis: boolean[][] = Array.from({ length: m }, () => Array(n).fill(false));
    const dfs = (x: number, y: number, px: number, py: number): boolean => {
        vis[x][y] = true;
        const dirs = [-1, 0, 1, 0, -1];
        for (let k = 0; k < 4; k++) {
            const [nx, ny] = [x + dirs[k], y + dirs[k + 1]];
            if (nx >= 0 && nx < m && ny >= 0 && ny < n) {
                if (grid[nx][ny] !== grid[x][y] || (nx === px && ny === py)) {
                    continue;
                }
                if (vis[nx][ny] || dfs(nx, ny, x, y)) {
                    return true;
                }
            }
        }
        return false;
    };
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (!vis[i][j] && dfs(i, j, -1, -1)) {
                return true;
            }
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn contains_cycle(grid: Vec<Vec<char>>) -> bool {
        let m = grid.len();
        let n = grid[0].len();
        let mut vis = vec![vec![false; n]; m];
        let dirs = vec![-1, 0, 1, 0, -1];

        fn dfs(
            x: usize,
            y: usize,
            px: isize,
            py: isize,
            grid: &Vec<Vec<char>>,
            vis: &mut Vec<Vec<bool>>,
            dirs: &Vec<isize>,
        ) -> bool {
            vis[x][y] = true;
            for k in 0..4 {
                let nx = (x as isize + dirs[k]) as usize;
                let ny = (y as isize + dirs[k + 1]) as usize;
                if nx < grid.len() && ny < grid[0].len() {
                    if grid[nx][ny] != grid[x][y] || (nx as isize == px && ny as isize == py) {
                        continue;
                    }
                    if vis[nx][ny] || dfs(nx, ny, x as isize, y as isize, grid, vis, dirs) {
                        return true;
                    }
                }
            }
            false
        }

        for i in 0..m {
            for j in 0..n {
                if !vis[i][j] && dfs(i, j, -1, -1, &grid, &mut vis, &dirs) {
                    return true;
                }
            }
        }
        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
