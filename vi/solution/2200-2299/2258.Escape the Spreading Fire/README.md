---
comments: true
difficulty: Hard
rating: 2346
source: Biweekly Contest 77 Q4
tags:
    - Breadth-First Search
    - Array
    - Binary Search
    - Matrix
---

<!-- problem:start -->

# [2258. Escape the Spreading Fire](https://leetcode.com/problems/escape-the-spreading-fire)

[中文文档](/solution/2200-2299/2258.Escape%20the%20Spreading%20Fire/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>grid</code> được đánh chỉ số từ <strong>0</strong>, có kích thước <code>m x n</code> và biểu diễn một cánh đồng. Mỗi ô có một trong ba giá trị sau:</p>

<ul>
	<li><code>0</code> biểu diễn cỏ,</li>
	<li><code>1</code> biểu diễn lửa,</li>
	<li><code>2</code> biểu diễn tường mà bạn và lửa không thể đi qua.</li>
</ul>

<p>Bạn đang ở ô trên cùng bên trái, <code>(0, 0)</code>, và muốn đi đến nơi trú ẩn an toàn ở ô dưới cùng bên phải, <code>(m - 1, n - 1)</code>. Mỗi phút, bạn có thể di chuyển đến một ô cỏ <strong>kề</strong>. <strong>Sau khi</strong> bạn di chuyển, mọi ô lửa sẽ lan sang tất cả các ô <strong>kề</strong> không phải là tường.</p>

<p>Hãy trả về <em><strong>số phút lớn nhất</strong> mà bạn có thể ở lại vị trí ban đầu trước khi di chuyển mà vẫn đến nơi trú ẩn an toàn</em>. Nếu không thể đến nơi đó, trả về <code>-1</code>. Nếu bạn <strong>luôn luôn</strong> có thể đến nơi trú ẩn an toàn bất kể đã ở lại bao lâu, trả về <code>10<sup>9</sup></code>.</p>

<p>Lưu ý rằng ngay cả khi lửa lan đến nơi trú ẩn an toàn ngay sau khi bạn đến đó, việc này vẫn được tính là đến nơi trú ẩn an toàn.</p>

<p>Một ô <strong>kề</strong> với ô khác nếu ô đó nằm ngay phía bắc, phía đông, phía nam hoặc phía tây của ô kia (tức là hai cạnh của chúng tiếp xúc nhau).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2258.Escape%20the%20Spreading%20Fire/images/ex1new.jpg" style="width: 650px; height: 404px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,2,0,0,0,0,0],[0,0,0,2,2,1,0],[0,2,0,0,1,2,0],[0,0,2,2,2,0,2],[0,0,0,0,0,0,0]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Hình phía trên minh họa tình huống bạn ở lại vị trí ban đầu trong 3 phút.
Bạn vẫn có thể đến nơi trú ẩn an toàn.
Ở lại hơn 3 phút sẽ khiến bạn không thể đến nơi trú ẩn an toàn.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2258.Escape%20the%20Spreading%20Fire/images/ex2new2.jpg" style="width: 515px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,0,0,0],[0,1,2,0],[0,2,0,0]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Hình phía trên minh họa tình huống bạn lập tức di chuyển về phía nơi trú ẩn an toàn.
Lửa sẽ lan đến mọi ô mà bạn di chuyển tới, nên không thể đến nơi trú ẩn an toàn.
Vì vậy, trả về -1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2258.Escape%20the%20Spreading%20Fire/images/ex3new.jpg" style="width: 174px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,0,0],[2,2,0],[1,2,0]]
<strong>Đầu ra:</strong> 1000000000
<strong>Giải thích:</strong> Đây là lưới ban đầu.
Nhận thấy lửa bị các bức tường ngăn lại, vì vậy bạn luôn có thể đến nơi trú ẩn an toàn.
Do đó, trả về 10<sup>9</sup>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 300</code></li>
	<li><code>4 &lt;= m * n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code>, <code>1</code> hoặc <code>2</code>.</li>
	<li><code>grid[0][0] == grid[m - 1][n - 1] == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể chờ $t$ phút trước khi di chuyển, trong khi lửa lan ra sau mỗi phút; mục tiêu là tìm $t$ lớn nhất sao cho vẫn có thể đến nơi an toàn. Lưới có nhiều nhất khoảng $2\times 10^4$ ô. Tính khả thi đơn điệu theo $t$, nên ta có thể dùng tìm kiếm nhị phân.
>
> Với một giá trị $t$ bất kỳ, ta cho lửa lan trong $t$ phút; nếu vị trí bắt đầu bị cháy thì thất bại. Sau đó, ta dùng BFS để di chuyển người đồng bộ với lửa, chỉ bước vào các ô cỏ chưa cháy. Nếu đến lối ra trước hoặc cùng lúc với lửa thì thành công. Nếu ngay cả $t=mn$ cũng khả thi, trả về $10^9$.

<!-- thinking:end -->

Ta nhận thấy nếu thời gian ở lại $t$ thỏa mãn điều kiện, thì mọi thời gian ở lại nhỏ hơn $t$ cũng thỏa mãn điều kiện. Do đó, ta có thể dùng tìm kiếm nhị phân để tìm thời gian ở lại lớn nhất thỏa mãn điều kiện.

Ta đặt biên trái của tìm kiếm nhị phân là $l=-1$ và biên phải là $r=m \times n$. Trong mỗi vòng lặp của tìm kiếm nhị phân, ta lấy điểm giữa $mid$ của $l$ và $r$ làm thời gian ở lại hiện tại và kiểm tra xem nó có thỏa mãn điều kiện hay không. Nếu có, ta cập nhật $l$ thành $mid$, ngược lại cập nhật $r$ thành $mid-1$. Cuối cùng, nếu $l=m \times n$, điều đó có nghĩa là không có thời gian ở lại nào thỏa mãn điều kiện, nên ta trả về $10^9$; nếu không, ta trả về $l$.

Vấn đề then chốt là xác định xem thời gian ở lại $t$ có thỏa mãn điều kiện hay không. Ta có thể dùng BFS để mô phỏng lửa lan ra trong $t$ đơn vị thời gian. Nếu lửa lan đến vị trí bắt đầu sau khi ở lại $t$ đơn vị thời gian, điều đó có nghĩa là điều kiện không được thỏa mãn và ta trả về ngay. Nếu không, ta lại dùng BFS, mỗi lần tìm kiếm theo bốn hướng từ vị trí hiện tại, đồng thời sau mỗi lượt cần cho lửa lan theo bốn hướng. Nếu tìm thấy đường đi từ vị trí bắt đầu đến vị trí kết thúc trong quá trình này, điều đó có nghĩa là điều kiện được thỏa mãn.

Độ phức tạp thời gian là $O(m \times n \times \log (m \times n))$, và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumMinutes(self, grid: List[List[int]]) -> int:
        def spread(q: Deque[int]) -> Deque[int]:
            nq = deque()
            while q:
                i, j = q.popleft()
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    if 0 <= x < m and 0 <= y < n and not fire[x][y] and grid[x][y] == 0:
                        fire[x][y] = True
                        nq.append((x, y))
            return nq

        def check(t: int) -> bool:
            for i in range(m):
                for j in range(n):
                    fire[i][j] = False
            q1 = deque()
            for i, row in enumerate(grid):
                for j, x in enumerate(row):
                    if x == 1:
                        fire[i][j] = True
                        q1.append((i, j))
            while t and q1:
                q1 = spread(q1)
                t -= 1
            if fire[0][0]:
                return False
            q2 = deque([(0, 0)])
            vis = [[False] * n for _ in range(m)]
            vis[0][0] = True
            while q2:
                for _ in range(len(q2)):
                    i, j = q2.popleft()
                    if fire[i][j]:
                        continue
                    for a, b in pairwise(dirs):
                        x, y = i + a, j + b
                        if (
                            0 <= x < m
                            and 0 <= y < n
                            and not vis[x][y]
                            and not fire[x][y]
                            and grid[x][y] == 0
                        ):
                            if x == m - 1 and y == n - 1:
                                return True
                            vis[x][y] = True
                            q2.append((x, y))
                q1 = spread(q1)
            return False

        m, n = len(grid), len(grid[0])
        l, r = -1, m * n
        dirs = (-1, 0, 1, 0, -1)
        fire = [[False] * n for _ in range(m)]
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return int(1e9) if l == m * n else l
```

#### Java

```java
class Solution {
    private int[][] grid;
    private boolean[][] fire;
    private boolean[][] vis;
    private final int[] dirs = {-1, 0, 1, 0, -1};
    private int m;
    private int n;

    public int maximumMinutes(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        fire = new boolean[m][n];
        vis = new boolean[m][n];
        int l = -1, r = m * n;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l == m * n ? 1000000000 : l;
    }

    private boolean check(int t) {
        for (int i = 0; i < m; ++i) {
            Arrays.fill(fire[i], false);
            Arrays.fill(vis[i], false);
        }
        Deque<int[]> q1 = new ArrayDeque<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    q1.offer(new int[] {i, j});
                    fire[i][j] = true;
                }
            }
        }
        for (; t > 0 && !q1.isEmpty(); --t) {
            q1 = spread(q1);
        }
        if (fire[0][0]) {
            return false;
        }
        Deque<int[]> q2 = new ArrayDeque<>();
        q2.offer(new int[] {0, 0});
        vis[0][0] = true;
        for (; !q2.isEmpty(); q1 = spread(q1)) {
            for (int d = q2.size(); d > 0; --d) {
                int[] p = q2.poll();
                if (fire[p[0]][p[1]]) {
                    continue;
                }
                for (int k = 0; k < 4; ++k) {
                    int x = p[0] + dirs[k], y = p[1] + dirs[k + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && !fire[x][y] && !vis[x][y]
                        && grid[x][y] == 0) {
                        if (x == m - 1 && y == n - 1) {
                            return true;
                        }
                        vis[x][y] = true;
                        q2.offer(new int[] {x, y});
                    }
                }
            }
        }
        return false;
    }

    private Deque<int[]> spread(Deque<int[]> q) {
        Deque<int[]> nq = new ArrayDeque<>();
        while (!q.isEmpty()) {
            int[] p = q.poll();
            for (int k = 0; k < 4; ++k) {
                int x = p[0] + dirs[k], y = p[1] + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && !fire[x][y] && grid[x][y] == 0) {
                    fire[x][y] = true;
                    nq.offer(new int[] {x, y});
                }
            }
        }
        return nq;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumMinutes(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        bool vis[m][n];
        bool fire[m][n];
        int dirs[5] = {-1, 0, 1, 0, -1};
        auto spread = [&](queue<pair<int, int>>& q) {
            queue<pair<int, int>> nq;
            while (q.size()) {
                auto [i, j] = q.front();
                q.pop();
                for (int k = 0; k < 4; ++k) {
                    int x = i + dirs[k], y = j + dirs[k + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && !fire[x][y] && grid[x][y] == 0) {
                        fire[x][y] = true;
                        nq.emplace(x, y);
                    }
                }
            }
            return nq;
        };
        auto check = [&](int t) {
            memset(vis, false, sizeof(vis));
            memset(fire, false, sizeof(fire));
            queue<pair<int, int>> q1;
            for (int i = 0; i < m; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (grid[i][j] == 1) {
                        q1.emplace(i, j);
                        fire[i][j] = true;
                    }
                }
            }
            for (; t && q1.size(); --t) {
                q1 = spread(q1);
            }
            if (fire[0][0]) {
                return false;
            }
            queue<pair<int, int>> q2;
            q2.emplace(0, 0);
            vis[0][0] = true;
            for (; q2.size(); q1 = spread(q1)) {
                for (int d = q2.size(); d; --d) {
                    auto [i, j] = q2.front();
                    q2.pop();
                    if (fire[i][j]) {
                        continue;
                    }
                    for (int k = 0; k < 4; ++k) {
                        int x = i + dirs[k], y = j + dirs[k + 1];
                        if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && !fire[x][y] && grid[x][y] == 0) {
                            if (x == m - 1 && y == n - 1) {
                                return true;
                            }
                            vis[x][y] = true;
                            q2.emplace(x, y);
                        }
                    }
                }
            }
            return false;
        };
        int l = -1, r = m * n;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l == m * n ? 1e9 : l;
    }
};
```

#### Go

```go
func maximumMinutes(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	fire := make([][]bool, m)
	vis := make([][]bool, m)
	dirs := [5]int{-1, 0, 1, 0, -1}
	for i := range fire {
		fire[i] = make([]bool, n)
		vis[i] = make([]bool, n)
	}
	l, r := -1, m*n
	spread := func(q [][2]int) [][2]int {
		nq := [][2]int{}
		for len(q) > 0 {
			p := q[0]
			q = q[1:]
			for k := 0; k < 4; k++ {
				x, y := p[0]+dirs[k], p[1]+dirs[k+1]
				if x >= 0 && x < m && y >= 0 && y < n && !fire[x][y] && grid[x][y] == 0 {
					fire[x][y] = true
					nq = append(nq, [2]int{x, y})
				}
			}
		}
		return nq
	}
	check := func(t int) bool {
		for i := range fire {
			for j := range fire[i] {
				fire[i][j] = false
				vis[i][j] = false
			}
		}
		q1 := [][2]int{}
		for i := 0; i < m; i++ {
			for j := 0; j < n; j++ {
				if grid[i][j] == 1 {
					q1 = append(q1, [2]int{i, j})
					fire[i][j] = true
				}
			}
		}
		for ; t > 0 && len(q1) > 0; t-- {
			q1 = spread(q1)
		}
		q2 := [][2]int{{0, 0}}
		vis[0][0] = true
		for ; len(q2) > 0; q1 = spread(q1) {
			for d := len(q2); d > 0; d-- {
				p := q2[0]
				q2 = q2[1:]
				if fire[p[0]][p[1]] {
					continue
				}
				for k := 0; k < 4; k++ {
					x, y := p[0]+dirs[k], p[1]+dirs[k+1]
					if x >= 0 && x < m && y >= 0 && y < n && !fire[x][y] && !vis[x][y] && grid[x][y] == 0 {
						if x == m-1 && y == n-1 {
							return true
						}
						vis[x][y] = true
						q2 = append(q2, [2]int{x, y})
					}
				}
			}
		}
		return false
	}
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	if l == m*n {
		return int(1e9)
	}
	return l
}
```

#### TypeScript

```ts
function maximumMinutes(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const fire = Array.from({ length: m }, () => Array.from({ length: n }, () => false));
    const vis = Array.from({ length: m }, () => Array.from({ length: n }, () => false));
    const dirs: number[] = [-1, 0, 1, 0, -1];
    let [l, r] = [-1, m * n];
    const spread = (q: number[][]): number[][] => {
        const nq: number[][] = [];
        while (q.length) {
            const [i, j] = q.shift()!;
            for (let k = 0; k < 4; ++k) {
                const [x, y] = [i + dirs[k], j + dirs[k + 1]];
                if (x >= 0 && x < m && y >= 0 && y < n && !fire[x][y] && grid[x][y] === 0) {
                    fire[x][y] = true;
                    nq.push([x, y]);
                }
            }
        }
        return nq;
    };
    const check = (t: number): boolean => {
        for (let i = 0; i < m; ++i) {
            fire[i].fill(false);
            vis[i].fill(false);
        }
        let q1: number[][] = [];
        for (let i = 0; i < m; ++i) {
            for (let j = 0; j < n; ++j) {
                if (grid[i][j] === 1) {
                    q1.push([i, j]);
                    fire[i][j] = true;
                }
            }
        }
        for (; t && q1.length; --t) {
            q1 = spread(q1);
        }
        if (fire[0][0]) {
            return false;
        }
        const q2: number[][] = [[0, 0]];
        vis[0][0] = true;
        for (; q2.length; q1 = spread(q1)) {
            for (let d = q2.length; d; --d) {
                const [i, j] = q2.shift()!;
                if (fire[i][j]) {
                    continue;
                }
                for (let k = 0; k < 4; ++k) {
                    const [x, y] = [i + dirs[k], j + dirs[k + 1]];
                    if (
                        x >= 0 &&
                        x < m &&
                        y >= 0 &&
                        y < n &&
                        !vis[x][y] &&
                        !fire[x][y] &&
                        grid[x][y] === 0
                    ) {
                        if (x === m - 1 && y === n - 1) {
                            return true;
                        }
                        vis[x][y] = true;
                        q2.push([x, y]);
                    }
                }
            }
        }
        return false;
    };
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l === m * n ? 1e9 : l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
