---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Array
    - Matrix
    - Shortest Path
    - Dijkstra
    - Heap (Priority Queue)
    - A* Search
    - Heuristic Search
---

<!-- problem:start -->

# [505. The Maze II 🔒](https://leetcode.com/problems/the-maze-ii)

[中文文档](/solution/0500-0599/0505.The%20Maze%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có một quả bóng trong <code>maze</code> gồm các ô trống (biểu diễn bằng <code>0</code>) và tường (biểu diễn bằng <code>1</code>). Bóng có thể lăn qua các ô trống theo hướng <strong>lên, xuống, trái hoặc phải</strong>, nhưng sẽ không dừng lại cho đến khi chạm tường. Khi dừng, bóng có thể chọn hướng tiếp theo.</p>

<p>Cho <code>maze</code> kích thước <code>m x n</code>, vị trí <code>start</code> của bóng và <code>destination</code>, trong đó <code>start = [start<sub>row</sub>, start<sub>col</sub>]</code> và <code>destination = [destination<sub>row</sub>, destination<sub>col</sub>]</code>. Hãy trả về <em><strong>khoảng cách</strong> ngắn nhất để bóng dừng tại đích</em>. Nếu bóng không thể dừng tại <code>destination</code>, hãy trả về <code>-1</code>.</p>

<p><strong>Khoảng cách</strong> là số lượng <strong>ô trống</strong> mà bóng đi qua từ vị trí bắt đầu (không tính) đến đích (có tính).</p>

<p>Có thể giả sử <strong>toàn bộ biên của mê cung đều là tường</strong> (xem ví dụ).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0505.The%20Maze%20II/images/maze1-1-grid.jpg" style="width: 573px; height: 573px;" />
<pre>
<strong>Đầu vào:</strong> maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Một đường đi có thể là: trái -&gt; xuống -&gt; trái -&gt; xuống -&gt; phải -&gt; xuống -&gt; phải.
Độ dài đường đi là 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0505.The%20Maze%20II/images/maze1-2-grid.jpg" style="width: 573px; height: 573px;" />
<pre>
<strong>Đầu vào:</strong> maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [3,2]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có cách nào để bóng dừng tại đích. Lưu ý rằng bóng có thể đi qua đích nhưng không thể dừng ở đó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> maze = [[0,0,0,0,0],[1,1,0,0,1],[0,0,0,0,0],[0,1,0,0,1],[0,1,0,0,0]], start = [4,3], destination = [0,1]
<strong>Đầu ra:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == maze.length</code></li>
	<li><code>n == maze[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>maze[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>start.length == 2</code></li>
	<li><code>destination.length == 2</code></li>
	<li><code>0 &lt;= start<sub>row</sub>, destination<sub>row</sub> &lt; m</code></li>
	<li><code>0 &lt;= start<sub>col</sub>, destination<sub>col</sub> &lt; n</code></li>
	<li>Cả bóng và đích đều nằm trên ô trống, và ban đầu chúng không ở cùng một vị trí.</li>
	<li>Mê cung có <strong>ít nhất 2 ô trống</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bóng lăn cho đến khi chạm tường, nên một cạnh tương ứng với cả một lần lăn chứ không phải một ô đơn lẻ. Di chuyển từng ô sẽ khiến ta phải xét lại cùng một lần lăn nhiều lần.
>
> Trạng thái là các ô bóng dừng lại. Từ mỗi ô dừng, cho bóng lăn theo bốn hướng đến điểm cuối rồi cập nhật khoảng cách của ô đó nếu tìm được đường đi ngắn hơn. Mỗi lần lăn có độ dài khác nhau, nên đây là bài toán cập nhật đường đi ngắn nhất trên các điểm dừng, không phải BFS không trọng số. Khoảng cách được ghi nhận tại đích là đáp án; nếu vẫn là vô cực thì trả về $-1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestDistance(
        self, maze: List[List[int]], start: List[int], destination: List[int]
    ) -> int:
        m, n = len(maze), len(maze[0])
        dirs = (-1, 0, 1, 0, -1)
        si, sj = start
        di, dj = destination
        q = deque([(si, sj)])
        dist = [[inf] * n for _ in range(m)]
        dist[si][sj] = 0
        while q:
            i, j = q.popleft()
            for a, b in pairwise(dirs):
                x, y, k = i, j, dist[i][j]
                while 0 <= x + a < m and 0 <= y + b < n and maze[x + a][y + b] == 0:
                    x, y, k = x + a, y + b, k + 1
                if k < dist[x][y]:
                    dist[x][y] = k
                    q.append((x, y))
        return -1 if dist[di][dj] == inf else dist[di][dj]
```

#### Java

```java
class Solution {
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int m = maze.length, n = maze[0].length;
        final int inf = 1 << 30;
        int[][] dist = new int[m][n];
        for (var row : dist) {
            Arrays.fill(row, inf);
        }
        int si = start[0], sj = start[1];
        int di = destination[0], dj = destination[1];
        dist[si][sj] = 0;
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {si, sj});
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            var p = q.poll();
            int i = p[0], j = p[1];
            for (int d = 0; d < 4; ++d) {
                int x = i, y = j, k = dist[i][j];
                int a = dirs[d], b = dirs[d + 1];
                while (
                    x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && maze[x + a][y + b] == 0) {
                    x += a;
                    y += b;
                    ++k;
                }
                if (k < dist[x][y]) {
                    dist[x][y] = k;
                    q.offer(new int[] {x, y});
                }
            }
        }
        return dist[di][dj] == inf ? -1 : dist[di][dj];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shortestDistance(vector<vector<int>>& maze, vector<int>& start, vector<int>& destination) {
        int m = maze.size(), n = maze[0].size();
        int dist[m][n];
        memset(dist, 0x3f, sizeof(dist));
        int si = start[0], sj = start[1];
        int di = destination[0], dj = destination[1];
        dist[si][sj] = 0;
        queue<pair<int, int>> q;
        q.emplace(si, sj);
        int dirs[5] = {-1, 0, 1, 0, -1};
        while (!q.empty()) {
            auto [i, j] = q.front();
            q.pop();
            for (int d = 0; d < 4; ++d) {
                int x = i, y = j, k = dist[i][j];
                int a = dirs[d], b = dirs[d + 1];
                while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && maze[x + a][y + b] == 0) {
                    x += a;
                    y += b;
                    ++k;
                }
                if (k < dist[x][y]) {
                    dist[x][y] = k;
                    q.emplace(x, y);
                }
            }
        }
        return dist[di][dj] == 0x3f3f3f3f ? -1 : dist[di][dj];
    }
};
```

#### Go

```go
func shortestDistance(maze [][]int, start []int, destination []int) int {
	m, n := len(maze), len(maze[0])
	dist := make([][]int, m)
	const inf = 1 << 30
	for i := range dist {
		dist[i] = make([]int, n)
		for j := range dist[i] {
			dist[i][j] = inf
		}
	}
	dist[start[0]][start[1]] = 0
	q := [][]int{start}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		i, j := p[0], p[1]
		for d := 0; d < 4; d++ {
			x, y, k := i, j, dist[i][j]
			a, b := dirs[d], dirs[d+1]
			for x+a >= 0 && x+a < m && y+b >= 0 && y+b < n && maze[x+a][y+b] == 0 {
				x, y, k = x+a, y+b, k+1
			}
			if k < dist[x][y] {
				dist[x][y] = k
				q = append(q, []int{x, y})
			}
		}
	}
	di, dj := destination[0], destination[1]
	if dist[di][dj] == inf {
		return -1
	}
	return dist[di][dj]
}
```

#### TypeScript

```ts
function shortestDistance(maze: number[][], start: number[], destination: number[]): number {
    const m = maze.length;
    const n = maze[0].length;
    const dist: number[][] = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Infinity),
    );
    const [si, sj] = start;
    const [di, dj] = destination;
    dist[si][sj] = 0;
    const q: number[][] = [[si, sj]];
    const dirs = [-1, 0, 1, 0, -1];
    while (q.length) {
        const [i, j] = q.shift()!;
        for (let d = 0; d < 4; ++d) {
            let [x, y, k] = [i, j, dist[i][j]];
            const [a, b] = [dirs[d], dirs[d + 1]];
            while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && maze[x + a][y + b] === 0) {
                x += a;
                y += b;
                ++k;
            }
            if (k < dist[x][y]) {
                dist[x][y] = k;
                q.push([x, y]);
            }
        }
    }
    return dist[di][dj] === Infinity ? -1 : dist[di][dj];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
