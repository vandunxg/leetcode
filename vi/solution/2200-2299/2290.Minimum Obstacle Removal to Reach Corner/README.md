---
comments: true
difficulty: Hard
rating: 2137
source: Weekly Contest 295 Q4
tags:
    - Breadth-First Search
    - Graph
    - Array
    - Matrix
    - Shortest Path
    - Dijkstra
    - Heap (Priority Queue)
    - 0-1 BFS
---

<!-- problem:start -->

# [2290. Minimum Obstacle Removal to Reach Corner](https://leetcode.com/problems/minimum-obstacle-removal-to-reach-corner)

[中文文档](/solution/2200-2299/2290.Minimum%20Obstacle%20Removal%20to%20Reach%20Corner/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>grid</code> được đánh chỉ số từ <strong>0</strong>, có kích thước <code>m x n</code>. Mỗi ô có một trong hai giá trị:</p>

<ul>
	<li><code>0</code> biểu diễn một ô <strong>trống</strong>,</li>
	<li><code>1</code> biểu diễn một <strong>chướng ngại vật</strong> có thể bị loại bỏ.</li>
</ul>

<p>Bạn có thể di chuyển lên, xuống, sang trái hoặc sang phải từ một ô trống và đến một ô trống.</p>

<p>Hãy trả về <em>số lượng <strong>nhỏ nhất</strong> <strong>chướng ngại vật</strong> cần <strong>loại bỏ</strong> để bạn có thể di chuyển từ góc trên bên trái </em><code>(0, 0)</code><em> đến góc dưới bên phải </em><code>(m - 1, n - 1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2290.Minimum%20Obstacle%20Removal%20to%20Reach%20Corner/images/example1drawio-1.png" style="width: 605px; height: 246px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,1],[1,1,0],[1,1,0]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể loại bỏ các chướng ngại vật tại (0, 1) và (0, 2) để tạo một đường đi từ (0, 0) đến (2, 2).
Có thể chứng minh rằng cần loại bỏ ít nhất 2 chướng ngại vật, nên ta trả về 2.
Lưu ý rằng có thể có những cách khác để loại bỏ 2 chướng ngại vật và tạo đường đi.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2290.Minimum%20Obstacle%20Removal%20to%20Reach%20Corner/images/example1drawio.png" style="width: 405px; height: 246px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Ta có thể di chuyển từ (0, 0) đến (2, 4) mà không cần loại bỏ chướng ngại vật nào, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> <strong>hoặc</strong> <code>1</code>.</li>
	<li><code>grid[0][0] == grid[m - 1][n - 1] == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS bằng hàng đợi hai đầu

<!-- thinking:start -->

> **Tư duy**
>
> Ta đi từ góc trên bên trái đến góc dưới bên phải; đi vào một chướng ngại vật có chi phí bằng một lần loại bỏ. Vì $mn \le 10^5$, ta không thể duyệt các tập con. Các ô trống có trọng số $0$ và chướng ngại vật có trọng số $1$, nên đây là bài toán đường đi ngắn nhất $0$-$1$, có thể giải bằng BFS với deque.
>
> Khi ô kề là ô trống, ta đưa nó vào đầu hàng đợi; khi đó là chướng ngại vật, ta đưa nó vào cuối hàng đợi. Lần đầu tiên đến ô đích chính là số lần loại bỏ nhỏ nhất.

<!-- thinking:end -->

Bài toán này về bản chất là mô hình đường đi ngắn nhất, nhưng ta cần tìm số lượng chướng ngại vật nhỏ nhất phải loại bỏ.

Trong một đồ thị vô hướng chỉ có các cạnh mang trọng số $0$ và $1$, ta có thể dùng hàng đợi hai đầu để thực hiện BFS. Nguyên tắc là nếu trọng số của điểm hiện tại có thể mở rộng bằng $0$, ta thêm điểm đó vào đầu hàng đợi; nếu trọng số bằng $1$, ta thêm vào cuối hàng đợi.

> Nếu trọng số của một cạnh bằng $0$, thì nút mới được mở rộng có cùng trọng số với nút ở đầu hàng đợi hiện tại, nên rõ ràng có thể dùng nó làm điểm bắt đầu cho lần mở rộng tiếp theo.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của grid.

Các bài tương tự:

- [1368. Minimum Cost to Make at Least One Valid Path in a Grid](https://github.com/doocs/leetcode/blob/main/solution/1300-1399/1368.Minimum%20Cost%20to%20Make%20at%20Least%20One%20Valid%20Path%20in%20a%20Grid/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumObstacles(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        q = deque([(0, 0, 0)])
        vis = set()
        dirs = (-1, 0, 1, 0, -1)
        while 1:
            i, j, k = q.popleft()
            if i == m - 1 and j == n - 1:
                return k
            if (i, j) in vis:
                continue
            vis.add((i, j))
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n:
                    if grid[x][y] == 0:
                        q.appendleft((x, y, k))
                    else:
                        q.append((x, y, k + 1))
```

#### Java

```java
class Solution {
    public int minimumObstacles(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {0, 0, 0});
        int[] dirs = {-1, 0, 1, 0, -1};
        boolean[][] vis = new boolean[m][n];
        while (true) {
            var p = q.poll();
            int i = p[0], j = p[1], k = p[2];
            if (i == m - 1 && j == n - 1) {
                return k;
            }
            if (vis[i][j]) {
                continue;
            }
            vis[i][j] = true;
            for (int h = 0; h < 4; ++h) {
                int x = i + dirs[h], y = j + dirs[h + 1];
                if (x >= 0 && x < m && y >= 0 && y < n) {
                    if (grid[x][y] == 0) {
                        q.offerFirst(new int[] {x, y, k});
                    } else {
                        q.offerLast(new int[] {x, y, k + 1});
                    }
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumObstacles(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        deque<tuple<int, int, int>> q{{0, 0, 0}};
        bool vis[m][n];
        memset(vis, 0, sizeof vis);
        int dirs[5] = {-1, 0, 1, 0, -1};
        while (1) {
            auto [i, j, k] = q.front();
            q.pop_front();
            if (i == m - 1 && j == n - 1) {
                return k;
            }
            if (vis[i][j]) {
                continue;
            }
            vis[i][j] = true;
            for (int h = 0; h < 4; ++h) {
                int x = i + dirs[h], y = j + dirs[h + 1];
                if (x >= 0 && x < m && y >= 0 && y < n) {
                    if (grid[x][y] == 0) {
                        q.push_front({x, y, k});
                    } else {
                        q.push_back({x, y, k + 1});
                    }
                }
            }
        }
    }
};
```

#### Go

```go
func minimumObstacles(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	q := doublylinkedlist.New()
	type tuple struct{ i, j, k int }
	q.Add(tuple{0, 0, 0})
	vis := make([][]bool, m)
	for i := range vis {
		vis[i] = make([]bool, n)
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for {
		v, _ := q.Get(0)
		p := v.(tuple)
		q.Remove(0)
		i, j, k := p.i, p.j, p.k
		if i == m-1 && j == n-1 {
			return k
		}
		if vis[i][j] {
			continue
		}
		vis[i][j] = true
		for h := 0; h < 4; h++ {
			x, y := i+dirs[h], j+dirs[h+1]
			if x >= 0 && x < m && y >= 0 && y < n {
				if grid[x][y] == 0 {
					q.Insert(0, tuple{x, y, k})
				} else {
					q.Add(tuple{x, y, k + 1})
				}
			}
		}
	}
}
```

#### TypeScript

```ts
function minimumObstacles(grid: number[][]): number {
    const m = grid.length,
        n = grid[0].length;
    const dirs = [
        [0, 1],
        [0, -1],
        [1, 0],
        [-1, 0],
    ];
    let ans = Array.from({ length: m }, v => new Array(n).fill(Infinity));
    ans[0][0] = 0;
    let deque = [[0, 0]];
    while (deque.length) {
        let [x, y] = deque.shift();
        for (let [dx, dy] of dirs) {
            let [i, j] = [x + dx, y + dy];
            if (i < 0 || i > m - 1 || j < 0 || j > n - 1) continue;
            const cost = grid[i][j];
            if (ans[x][y] + cost >= ans[i][j]) continue;
            ans[i][j] = ans[x][y] + cost;
            deque.push([i, j]);
        }
    }
    return ans[m - 1][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
