---
comments: true
difficulty: Hard
rating: 2068
source: Weekly Contest 178 Q4
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

# [1368. Minimum Cost to Make at Least One Valid Path in a Grid](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid)

[中文文档](/solution/1300-1399/1368.Minimum%20Cost%20to%20Make%20at%20Least%20One%20Valid%20Path%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới <code>m x n</code>. Mỗi ô có một mũi tên chỉ đến ô tiếp theo cần đi nếu bạn đang ở ô đó. Hướng của <code>grid[i][j]</code> có thể là:</p>

<ul>
	<li><code>1</code> nghĩa là đi sang ô bên phải (tức đi từ <code>grid[i][j]</code> đến <code>grid[i][j + 1]</code>).</li>
	<li><code>2</code> nghĩa là đi sang ô bên trái (tức đi từ <code>grid[i][j]</code> đến <code>grid[i][j - 1]</code>).</li>
	<li><code>3</code> nghĩa là đi xuống ô bên dưới (tức đi từ <code>grid[i][j]</code> đến <code>grid[i + 1][j]</code>).</li>
	<li><code>4</code> nghĩa là đi lên ô bên trên (tức đi từ <code>grid[i][j]</code> đến <code>grid[i - 1][j]</code>).</li>
</ul>

<p>Lưu ý một số mũi tên trong lưới có thể chỉ ra ngoài lưới.</p>

<p>Bạn bắt đầu tại ô trên cùng bên trái <code>(0, 0)</code>. Một đường đi hợp lệ trong lưới bắt đầu từ ô này và kết thúc tại ô dưới cùng bên phải <code>(m - 1, n - 1)</code>, đồng thời đi theo các mũi tên trong lưới. Đường đi hợp lệ không nhất thiết phải ngắn nhất.</p>

<p>Bạn có thể đổi hướng mũi tên tại một ô với <code>cost = 1</code>. Mỗi ô chỉ được đổi mũi tên <strong>một lần duy nhất</strong>.</p>

<p>Trả về <em>chi phí nhỏ nhất để lưới có ít nhất một đường đi hợp lệ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1368.Minimum%20Cost%20to%20Make%20at%20Least%20One%20Valid%20Path%20in%20a%20Grid/images/grid1.png" style="width: 400px; height: 390px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1,1],[2,2,2,2],[1,1,1,1],[2,2,2,2]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Bạn bắt đầu tại ô (0, 0).
Đường đi đến (3, 3) như sau: (0, 0) --&gt; (0, 1) --&gt; (0, 2) --&gt; (0, 3), đổi mũi tên hướng xuống với chi phí = 1 --&gt; (1, 3) --&gt; (1, 2) --&gt; (1, 1) --&gt; (1, 0), đổi mũi tên hướng xuống với chi phí = 1 --&gt; (2, 0) --&gt; (2, 1) --&gt; (2, 2) --&gt; (2, 3), đổi mũi tên hướng xuống với chi phí = 1 --&gt; (3, 3)
Tổng chi phí = 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1368.Minimum%20Cost%20to%20Make%20at%20Least%20One%20Valid%20Path%20in%20a%20Grid/images/grid2.png" style="width: 350px; height: 341px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,3],[3,2,2],[1,1,4]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Bạn có thể đi theo các mũi tên từ (0, 0) đến (2, 2).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1368.Minimum%20Cost%20to%20Make%20at%20Least%20One%20Valid%20Path%20in%20a%20Grid/images/grid3.png" style="width: 200px; height: 192px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2],[4,3]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 4</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS với deque

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ô chỉ đến một ô kề bên; đổi mũi tên tốn $1$. Ta cần số lần đổi ít nhất để đến ô dưới cùng bên phải. Dijkstra có độ phức tạp $O(mn\log)$. Trọng số cạnh chỉ có thể là $0$ (đi theo mũi tên) hoặc $1$ (đổi hướng), nên có thể dùng 0-1 BFS: nếu mũi tên khớp thì đưa ô kế tiếp lên đầu deque với cùng chi phí; nếu phải đổi thì đưa vào cuối deque với chi phí tăng thêm một. Lần đầu đến đích sẽ cho chi phí tối ưu.

<!-- thinking:end -->

Bài toán này về bản chất là tìm đường đi ngắn nhất, nhưng đại lượng cần tối thiểu hóa là số lần đổi hướng.

Trong đồ thị vô hướng có trọng số cạnh chỉ là 0 hoặc 1, ta có thể dùng deque để BFS. Nguyên tắc là nếu bước mở rộng hiện tại có trọng số 0 thì thêm node mới vào đầu deque; nếu trọng số là 1 thì thêm node đó vào cuối deque.

> Nếu cạnh có trọng số 0, thì trọng số của node mới được mở rộng bằng trọng số của node đầu queue hiện tại. Vì vậy, node mới có thể được dùng làm điểm bắt đầu cho lượt mở rộng tiếp theo.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        dirs = [[0, 0], [0, 1], [0, -1], [1, 0], [-1, 0]]
        q = deque([(0, 0, 0)])
        vis = set()
        while q:
            i, j, d = q.popleft()
            if (i, j) in vis:
                continue
            vis.add((i, j))
            if i == m - 1 and j == n - 1:
                return d
            for k in range(1, 5):
                x, y = i + dirs[k][0], j + dirs[k][1]
                if 0 <= x < m and 0 <= y < n:
                    if grid[i][j] == k:
                        q.appendleft((x, y, d))
                    else:
                        q.append((x, y, d + 1))
        return -1
```

#### Java

```java
class Solution {
    public int minCost(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        boolean[][] vis = new boolean[m][n];
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {0, 0, 0});
        int[][] dirs = {{0, 0}, {0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int i = p[0], j = p[1], d = p[2];
            if (i == m - 1 && j == n - 1) {
                return d;
            }
            if (vis[i][j]) {
                continue;
            }
            vis[i][j] = true;
            for (int k = 1; k <= 4; ++k) {
                int x = i + dirs[k][0], y = j + dirs[k][1];
                if (x >= 0 && x < m && y >= 0 && y < n) {
                    if (grid[i][j] == k) {
                        q.offerFirst(new int[] {x, y, d});
                    } else {
                        q.offer(new int[] {x, y, d + 1});
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
    int minCost(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<bool>> vis(m, vector<bool>(n));
        vector<vector<int>> dirs = {{0, 0}, {0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        deque<pair<int, int>> q;
        q.push_back({0, 0});
        while (!q.empty()) {
            auto p = q.front();
            q.pop_front();
            int i = p.first / n, j = p.first % n, d = p.second;
            if (i == m - 1 && j == n - 1) return d;
            if (vis[i][j]) continue;
            vis[i][j] = true;
            for (int k = 1; k <= 4; ++k) {
                int x = i + dirs[k][0], y = j + dirs[k][1];
                if (x >= 0 && x < m && y >= 0 && y < n) {
                    if (grid[i][j] == k)
                        q.push_front({x * n + y, d});
                    else
                        q.push_back({x * n + y, d + 1});
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minCost(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	q := doublylinkedlist.New()
	q.Add([]int{0, 0, 0})
	dirs := [][]int{{0, 0}, {0, 1}, {0, -1}, {1, 0}, {-1, 0}}
	vis := make([][]bool, m)
	for i := range vis {
		vis[i] = make([]bool, n)
	}
	for !q.Empty() {
		v, _ := q.Get(0)
		p := v.([]int)
		q.Remove(0)
		i, j, d := p[0], p[1], p[2]
		if i == m-1 && j == n-1 {
			return d
		}
		if vis[i][j] {
			continue
		}
		vis[i][j] = true
		for k := 1; k <= 4; k++ {
			x, y := i+dirs[k][0], j+dirs[k][1]
			if x >= 0 && x < m && y >= 0 && y < n {
				if grid[i][j] == k {
					q.Insert(0, []int{x, y, d})
				} else {
					q.Add([]int{x, y, d + 1})
				}
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minCost(grid: number[][]): number {
    const m = grid.length,
        n = grid[0].length;
    let ans = Array.from({ length: m }, v => new Array(n).fill(Infinity));
    ans[0][0] = 0;
    let queue = [[0, 0]];
    const dirs = [
        [0, 1],
        [0, -1],
        [1, 0],
        [-1, 0],
    ];
    while (queue.length) {
        let [x, y] = queue.shift();
        for (let step = 1; step < 5; step++) {
            let [dx, dy] = dirs[step - 1];
            let [i, j] = [x + dx, y + dy];
            if (i < 0 || i >= m || j < 0 || j >= n) continue;
            let cost = ~~(grid[x][y] != step) + ans[x][y];
            if (cost >= ans[i][j]) continue;
            ans[i][j] = cost;
            if (grid[x][y] == step) {
                queue.unshift([i, j]);
            } else {
                queue.push([i, j]);
            }
        }
    }
    return ans[m - 1][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
