---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Array
    - String
    - Matrix
    - Shortest Path
    - Dijkstra
    - Heap (Priority Queue)
    - A* Search
    - Heuristic Search
---

<!-- problem:start -->

# [499. The Maze III 🔒](https://leetcode.com/problems/the-maze-iii)

[中文文档](/solution/0400-0499/0499.The%20Maze%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Có một quả bóng trong <code>maze</code> gồm các ô trống (biểu diễn bằng <code>0</code>) và tường (biểu diễn bằng <code>1</code>). Bóng có thể lăn qua các ô trống theo hướng <strong>lên, xuống, trái hoặc phải</strong>, nhưng chỉ dừng lại khi chạm tường. Khi dừng, bóng có thể chọn hướng tiếp theo (phải khác hướng vừa chọn). Trong mê cung còn có một lỗ; bóng sẽ rơi xuống lỗ nếu lăn tới đó.</p>

<p>Cho <code>maze</code> kích thước <code>m x n</code>, vị trí của bóng <code>ball</code> và vị trí lỗ <code>hole</code>, trong đó <code>ball = [ball<sub>row</sub>, ball<sub>col</sub>]</code> và <code>hole = [hole<sub>row</sub>, hole<sub>col</sub>]</code>. Hãy trả về chuỗi chỉ dẫn <code>instructions</code> để bóng đi vào lỗ với <strong>quãng đường ngắn nhất</strong>. Nếu có nhiều chuỗi chỉ dẫn hợp lệ, trả về chuỗi nhỏ nhất theo thứ tự từ điển. Nếu bóng không thể vào lỗ, trả về <code>&quot;impossible&quot;</code>.</p>

<p>Nếu bóng có thể vào lỗ, chuỗi <code>instructions</code> chỉ gồm các ký tự <code>&#39;u&#39;</code> (lên), <code>&#39;d&#39;</code> (xuống), <code>&#39;l&#39;</code> (trái) và <code>&#39;r&#39;</code> (phải).</p>

<p><strong>Quãng đường</strong> là số <strong>ô trống</strong> bóng đi qua, không tính vị trí bắt đầu nhưng tính vị trí đích.</p>

<p>Có thể giả định <strong>toàn bộ biên của mê cung đều là tường</strong> (xem ví dụ).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0499.The%20Maze%20III/images/maze3-1-grid.jpg" style="width: 573px; height: 573px;" />
<pre>
<strong>Đầu vào:</strong> maze = [[0,0,0,0,0],[1,1,0,0,1],[0,0,0,0,0],[0,1,0,0,1],[0,1,0,0,0]], ball = [4,3], hole = [0,1]
<strong>Đầu ra:</strong> &quot;lul&quot;
<strong>Giải thích:</strong> Có hai cách ngắn nhất để bóng vào lỗ.
Cách thứ nhất là trái -&gt; lên -&gt; trái, biểu diễn bằng &quot;lul&quot;.
Cách thứ hai là lên -&gt; trái, biểu diễn bằng &#39;ul&#39;.
Cả hai đều có quãng đường ngắn nhất bằng 6, nhưng cách thứ nhất nhỏ hơn theo thứ tự từ điển vì &#39;l&#39; &lt; &#39;u&#39;. Do đó, kết quả là &quot;lul&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0499.The%20Maze%20III/images/maze3-2-grid.jpg" style="width: 573px; height: 573px;" />
<pre>
<strong>Đầu vào:</strong> maze = [[0,0,0,0,0],[1,1,0,0,1],[0,0,0,0,0],[0,1,0,0,1],[0,1,0,0,0]], ball = [4,3], hole = [3,0]
<strong>Đầu ra:</strong> &quot;impossible&quot;
<strong>Giải thích:</strong> Bóng không thể đến được lỗ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> maze = [[0,0,0,0,0,0,0],[0,0,1,0,0,1,0],[0,0,0,0,1,0,0],[0,0,0,0,0,0,1]], ball = [0,4], hole = [3,5]
<strong>Đầu ra:</strong> &quot;dldr&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == maze.length</code></li>
	<li><code>n == maze[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>maze[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>ball.length == 2</code></li>
	<li><code>hole.length == 2</code></li>
	<li><code>0 &lt;= ball<sub>row</sub>, hole<sub>row</sub> &lt;= m</code></li>
	<li><code>0 &lt;= ball<sub>col</sub>, hole<sub>col</sub> &lt;= n</code></li>
	<li>Cả bóng và lỗ đều nằm trên ô trống, và ban đầu chúng không ở cùng vị trí.</li>
	<li>Mê cung có <strong>ít nhất 2 ô trống</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bóng dừng lại ở tường hoặc lỗ; cần tìm đường ngắn nhất đến lỗ, nếu hòa thì chọn chuỗi bước đi nhỏ nhất theo thứ tự từ điển. BFS chỉ dựa trên khoảng cách là chưa đủ.
>
> Với mỗi điểm dừng, lưu khoảng cách tốt nhất và đường đi tương ứng. Thử lăn theo bốn hướng và dừng ngay nếu bóng chạm lỗ. Cập nhật khi khoảng cách mới ngắn hơn, hoặc bằng nhau nhưng đường đi nhỏ hơn; thêm điểm dừng vào queue trừ khi đó là lỗ.
>
> Lỗ không phải tường, vì vậy vòng lặp lăn phải kiểm tra vị trí lỗ. Cập nhật đồng thời chuỗi đường đi và khoảng cách sẽ đảm bảo cả hai tiêu chí.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findShortestWay(
        self, maze: List[List[int]], ball: List[int], hole: List[int]
    ) -> str:
        m, n = len(maze), len(maze[0])
        r, c = ball
        rh, ch = hole
        q = deque([(r, c)])
        dist = [[inf] * n for _ in range(m)]
        dist[r][c] = 0
        path = [[None] * n for _ in range(m)]
        path[r][c] = ''
        while q:
            i, j = q.popleft()
            for a, b, d in [(-1, 0, 'u'), (1, 0, 'd'), (0, -1, 'l'), (0, 1, 'r')]:
                x, y, step = i, j, dist[i][j]
                while (
                    0 <= x + a < m
                    and 0 <= y + b < n
                    and maze[x + a][y + b] == 0
                    and (x != rh or y != ch)
                ):
                    x, y = x + a, y + b
                    step += 1
                if dist[x][y] > step or (
                    dist[x][y] == step and path[i][j] + d < path[x][y]
                ):
                    dist[x][y] = step
                    path[x][y] = path[i][j] + d
                    if x != rh or y != ch:
                        q.append((x, y))
        return path[rh][ch] or 'impossible'
```

#### Java

```java
class Solution {
    public String findShortestWay(int[][] maze, int[] ball, int[] hole) {
        int m = maze.length;
        int n = maze[0].length;
        int r = ball[0], c = ball[1];
        int rh = hole[0], ch = hole[1];
        Deque<int[]> q = new LinkedList<>();
        q.offer(new int[] {r, c});
        int[][] dist = new int[m][n];
        for (int i = 0; i < m; ++i) {
            Arrays.fill(dist[i], Integer.MAX_VALUE);
        }
        dist[r][c] = 0;
        String[][] path = new String[m][n];
        path[r][c] = "";
        int[][] dirs = {{-1, 0, 'u'}, {1, 0, 'd'}, {0, -1, 'l'}, {0, 1, 'r'}};
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int i = p[0], j = p[1];
            for (int[] dir : dirs) {
                int a = dir[0], b = dir[1];
                String d = String.valueOf((char) (dir[2]));
                int x = i, y = j;
                int step = dist[i][j];
                while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && maze[x + a][y + b] == 0
                    && (x != rh || y != ch)) {
                    x += a;
                    y += b;
                    ++step;
                }
                if (dist[x][y] > step
                    || (dist[x][y] == step && (path[i][j] + d).compareTo(path[x][y]) < 0)) {
                    dist[x][y] = step;
                    path[x][y] = path[i][j] + d;
                    if (x != rh || y != ch) {
                        q.offer(new int[] {x, y});
                    }
                }
            }
        }
        return path[rh][ch] == null ? "impossible" : path[rh][ch];
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findShortestWay(vector<vector<int>>& maze, vector<int>& ball, vector<int>& hole) {
        int m = maze.size();
        int n = maze[0].size();
        int r = ball[0], c = ball[1];
        int rh = hole[0], ch = hole[1];
        queue<pair<int, int>> q;
        q.push({r, c});
        vector<vector<int>> dist(m, vector<int>(n, INT_MAX));
        dist[r][c] = 0;
        vector<vector<string>> path(m, vector<string>(n, ""));
        vector<vector<int>> dirs = {{-1, 0, 'u'}, {1, 0, 'd'}, {0, -1, 'l'}, {0, 1, 'r'}};
        while (!q.empty()) {
            auto p = q.front();
            q.pop();
            int i = p.first, j = p.second;
            for (auto& dir : dirs) {
                int a = dir[0], b = dir[1];
                char d = (char) dir[2];
                int x = i, y = j;
                int step = dist[i][j];
                while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && maze[x + a][y + b] == 0 && (x != rh || y != ch)) {
                    x += a;
                    y += b;
                    ++step;
                }
                if (dist[x][y] > step || (dist[x][y] == step && (path[i][j] + d < path[x][y]))) {
                    dist[x][y] = step;
                    path[x][y] = path[i][j] + d;
                    if (x != rh || y != ch) q.push({x, y});
                }
            }
        }
        return path[rh][ch] == "" ? "impossible" : path[rh][ch];
    }
};
```

#### Go

```go
import "math"

func findShortestWay(maze [][]int, ball []int, hole []int) string {
	m, n := len(maze), len(maze[0])
	r, c := ball[0], ball[1]
	rh, ch := hole[0], hole[1]
	q := [][]int{[]int{r, c}}
	dist := make([][]int, m)
	path := make([][]string, m)
	for i := range dist {
		dist[i] = make([]int, n)
		path[i] = make([]string, n)
		for j := range dist[i] {
			dist[i][j] = math.MaxInt32
			path[i][j] = ""
		}
	}
	dist[r][c] = 0
	dirs := map[string][]int{"u": {-1, 0}, "d": {1, 0}, "l": {0, -1}, "r": {0, 1}}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		i, j := p[0], p[1]
		for d, dir := range dirs {
			a, b := dir[0], dir[1]
			x, y := i, j
			step := dist[i][j]
			for x+a >= 0 && x+a < m && y+b >= 0 && y+b < n && maze[x+a][y+b] == 0 && (x != rh || y != ch) {
				x += a
				y += b
				step++
			}
			if dist[x][y] > step || (dist[x][y] == step && (path[i][j]+d) < path[x][y]) {
				dist[x][y] = step
				path[x][y] = path[i][j] + d
				if x != rh || y != ch {
					q = append(q, []int{x, y})
				}
			}
		}
	}
	if path[rh][ch] == "" {
		return "impossible"
	}
	return path[rh][ch]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
