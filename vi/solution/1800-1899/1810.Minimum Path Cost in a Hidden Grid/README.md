---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Array
    - Interactive
    - Matrix
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1810. Minimum Path Cost in a Hidden Grid 🔒](https://leetcode.com/problems/minimum-path-cost-in-a-hidden-grid)

[中文文档](/solution/1800-1899/1810.Minimum%20Path%20Cost%20in%20a%20Hidden%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Đây là một <strong>bài toán tương tác</strong>.</p>

<p>Có một robot trong một lưới bị ẩn và bạn cần đưa robot từ ô bắt đầu đến ô đích trong lưới này. Lưới có kích thước <code>m x n</code>, mỗi ô trong lưới là ô trống hoặc bị chặn. Đề bài <strong>đảm bảo</strong> ô bắt đầu và ô đích khác nhau, đồng thời cả hai đều không bị chặn.</p>

<p>Mỗi ô có một <strong>chi phí</strong> mà bạn phải trả mỗi khi <strong>di chuyển</strong> đến ô đó. Chi phí của ô bắt đầu <strong>không</strong> được áp dụng trước khi robot di chuyển.</p>

<p>Bạn cần tìm tổng chi phí nhỏ nhất để đưa robot đến ô đích. Tuy nhiên, bạn <strong>không biết</strong> kích thước lưới, ô bắt đầu hay ô đích. Bạn chỉ được phép gửi truy vấn tới đối tượng <code>GridMaster</code>.</p>

<p>Lớp <code>GridMaster</code> có các hàm sau:</p>

<ul>
	<li><code>boolean canMove(char direction)</code> Trả về <code>true</code> nếu robot có thể di chuyển theo hướng đó. Nếu không, trả về <code>false</code>.</li>
	<li><code>int move(char direction)</code> Di chuyển robot theo hướng đó và trả về chi phí di chuyển đến ô mới. Nếu thao tác này đưa robot đến ô bị chặn hoặc ra ngoài lưới, thao tác sẽ bị <strong>bỏ qua</strong>, robot giữ nguyên vị trí và hàm trả về <code>-1</code>.</li>
	<li><code>boolean isTarget()</code> Trả về <code>true</code> nếu robot hiện đang ở ô đích. Nếu không, trả về <code>false</code>.</li>
</ul>

<p>Lưu ý rằng <code>direction</code> trong các hàm trên phải là một ký tự thuộc <code>{&#39;U&#39;,&#39;D&#39;,&#39;L&#39;,&#39;R&#39;}</code>, lần lượt biểu diễn hướng lên, xuống, trái và phải.</p>

<p>Trả về <em><strong>tổng chi phí nhỏ nhất</strong> để đưa robot từ ô bắt đầu đến ô đích. Nếu không có đường đi hợp lệ giữa hai ô, trả về </em><code>-1</code>.</p>

<p><strong>Kiểm thử tùy chỉnh:</strong></p>

<p>Đầu vào kiểm thử được đọc là ma trận 2D <code>grid</code> kích thước <code>m x n</code> và bốn số nguyên <code>r1</code>, <code>c1</code>, <code>r2</code> và <code><font face="monospace">c2</font></code>, trong đó:</p>

<ul>
	<li><code>grid[i][j] == 0</code> cho biết ô <code>(i, j)</code> bị chặn.</li>
	<li><code>grid[i][j] &gt;= 1</code> cho biết ô <code>(i, j)</code> trống và <code>grid[i][j]</code> là <strong>chi phí</strong> để di chuyển đến ô đó.</li>
	<li><code>(r1, c1)</code> là ô bắt đầu của robot.</li>
	<li><code>(r2, c2)</code> là ô đích của robot.</li>
</ul>

<p>Hãy nhớ rằng trong code của bạn sẽ <strong>không có</strong> thông tin này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[2,3],[1,1]], r1 = 0, c1 = 1, r2 = 1, c2 = 0
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Một tương tác có thể xảy ra được mô tả dưới đây:
Ban đầu robot đứng ở ô (0, 1), được biểu diễn bằng số 3.
 - master.canMove(&#39;U&#39;) trả về false.
 - master.canMove(&#39;D&#39;) trả về true.
 - master.canMove(&#39;L&#39;) trả về true.
 - master.canMove(&#39;R&#39;) trả về false.
 - master.move(&#39;L&#39;) đưa robot đến ô (0, 0) và trả về 2.
 - master.isTarget() trả về false.
 - master.canMove(&#39;U&#39;) trả về false.
 - master.canMove(&#39;D&#39;) trả về true.
 - master.canMove(&#39;L&#39;) trả về false.
 - master.canMove(&#39;R&#39;) trả về true.
 - master.move(&#39;D&#39;) đưa robot đến ô (1, 0) và trả về 1.
 - master.isTarget() trả về true.
 - master.move(&#39;L&#39;) không di chuyển robot và trả về -1.
 - master.move(&#39;R&#39;) đưa robot đến ô (1, 1) và trả về 1.
Ta biết ô đích là ô (1, 0), và tổng chi phí nhỏ nhất để đến đó là 2. </pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,3,1],[3,4,2],[1,2,0]], r1 = 2, c1 = 0, r2 = 0, c2 = 2
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Đường đi có chi phí nhỏ nhất là (2,0) -&gt; (2,1) -&gt; (1,1) -&gt; (1,2) -&gt; (0,2).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0],[0,1]], r1 = 0, c1 = 0, r2 = 1, c2 = 1
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có đường đi từ robot đến ô đích.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, m &lt;= 100</code></li>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dựng đồ thị bằng DFS + Thuật toán Dijkstra tối ưu bằng heap

<!-- thinking:start -->

> **Tư duy**
>
> Lưới bị ẩn; ta chỉ có thể thăm dò bằng $\textit{canMove}$, $\textit{move}$ và $\textit{isTarget}$, trong khi mỗi bước có chi phí. Tìm đường ngắn nhất ngay trong lúc khám phá sẽ trộn chi phí khám phá với chi phí thật của đường đi.
>
> Lưới có kích thước tối đa $100\times 100$, nên đặt ô bắt đầu tại $(100,100)$, dùng DFS cùng các bước đi ngược để dựng lại đồ thị, lưu chi phí đi vào mỗi ô và ghi nhận ô đích. Trọng số cạnh không âm, vì vậy Dijkstra từ ô bắt đầu cho chi phí nhỏ nhất; nếu DFS không gặp ô đích, trả về $-1$.

<!-- thinking:end -->

Ta nhận thấy kích thước lưới là $m \times n$, trong đó $m, n \leq 100$. Vì vậy, ta có thể khởi tạo tọa độ bắt đầu là $(sx, sy) = (100, 100)$ và giả sử lưới có kích thước $200 \times 200$. Sau đó, ta dùng tìm kiếm theo chiều sâu (DFS) để khám phá toàn bộ lưới và xây dựng mảng 2D $g$, trong đó $g[i][j]$ biểu diễn chi phí di chuyển từ điểm bắt đầu $(sx, sy)$ đến tọa độ $(i, j)$. Nếu một ô không thể đi tới, ta đặt giá trị của nó là $-1$. Ta lưu tọa độ đích trong $\textit{target}$; nếu không thể đi tới đích thì $\textit{target} = (-1, -1)$.

Tiếp theo, ta dùng thuật toán Dijkstra tối ưu bằng heap để tính đường đi có chi phí nhỏ nhất từ điểm bắt đầu $(sx, sy)$ đến đích $\textit{target}$. Ta dùng priority queue để lưu chi phí đường đi hiện tại và tọa độ, đồng thời dùng mảng 2D $\textit{dist}$ để ghi nhận chi phí nhỏ nhất từ điểm bắt đầu đến mỗi ô. Khi lấy một node khỏi priority queue, nếu node đó là đích, ta trả về chi phí đường đi hiện tại làm đáp án. Nếu chi phí đường đi của node lớn hơn giá trị được ghi trong $\textit{dist}$, ta bỏ qua node đó. Ngược lại, ta duyệt bốn ô kề của node. Nếu một ô kề có thể đi tới và chi phí đi qua node hiện tại nhỏ hơn, ta cập nhật chi phí của ô đó rồi thêm nó vào priority queue.

Độ phức tạp thời gian là $O(m \times n \log(m \times n))$ và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
# """
# This is GridMaster's API interface.
# You should not implement it, or speculate about its implementation
# """
# class GridMaster(object):
#    def canMove(self, direction: str) -> bool:
#
#
#    def move(self, direction: str) -> int:
#
#
#    def isTarget(self) -> bool:
#
#


class Solution(object):
    def findShortestPath(self, master: "GridMaster") -> int:
        def dfs(x: int, y: int) -> None:
            nonlocal target
            if master.isTarget():
                target = (x, y)
            for k in range(4):
                dx, dy = dirs[k], dirs[k + 1]
                nx, ny = x + dx, y + dy
                if (
                    0 <= nx < m
                    and 0 <= ny < n
                    and g[nx][ny] == -1
                    and master.canMove(s[k])
                ):
                    g[nx][ny] = master.move(s[k])
                    dfs(nx, ny)
                    master.move(s[(k + 2) % 4])

        dirs = (-1, 0, 1, 0, -1)
        s = "URDL"
        m = n = 200
        g = [[-1] * n for _ in range(m)]
        target = (-1, -1)
        sx = sy = 100
        dfs(sx, sy)
        if target == (-1, -1):
            return -1
        pq = [(0, sx, sy)]
        dist = [[inf] * n for _ in range(m)]
        dist[sx][sy] = 0
        while pq:
            w, x, y = heappop(pq)
            if (x, y) == target:
                return w
            for dx, dy in pairwise(dirs):
                nx, ny = x + dx, y + dy
                if (
                    0 <= nx < m
                    and 0 <= ny < n
                    and g[nx][ny] != -1
                    and w + g[nx][ny] < dist[nx][ny]
                ):
                    dist[nx][ny] = w + g[nx][ny]
                    heappush(pq, (dist[nx][ny], nx, ny))
        return -1
```

#### Java

```java
/**
 * // This is the GridMaster's API interface.
 * // You should not implement it, or speculate about its implementation
 * class GridMaster {
 *     boolean canMove(char direction);
 *     int move(char direction);
 *     boolean isTarget();
 * }
 */

class Solution {
    private final int m = 200;
    private final int n = 200;
    private final int inf = Integer.MAX_VALUE / 2;
    private final int[] dirs = {-1, 0, 1, 0, -1};
    private final char[] s = {'U', 'R', 'D', 'L'};
    private int[][] g;
    private int sx = 100, sy = 100;
    private int tx = -1, ty = -1;
    private GridMaster master;

    public int findShortestPath(GridMaster master) {
        this.master = master;
        g = new int[m][n];
        for (var gg : g) {
            Arrays.fill(gg, -1);
        }
        dfs(sx, sy);
        if (tx == -1 && ty == -1) {
            return -1;
        }
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {0, sx, sy});
        int[][] dist = new int[m][n];
        for (var gg : dist) {
            Arrays.fill(gg, inf);
        }
        dist[sx][sy] = 0;
        while (!pq.isEmpty()) {
            var p = pq.poll();
            int w = p[0], x = p[1], y = p[2];
            if (x == tx && y == ty) {
                return w;
            }
            if (w > dist[x][y]) {
                continue;
            }
            for (int k = 0; k < 4; ++k) {
                int nx = x + dirs[k], ny = y + dirs[k + 1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && g[nx][ny] != -1
                    && w + g[nx][ny] < dist[nx][ny]) {
                    dist[nx][ny] = w + g[nx][ny];
                    pq.offer(new int[] {dist[nx][ny], nx, ny});
                }
            }
        }
        return -1;
    }

    private void dfs(int x, int y) {
        if (master.isTarget()) {
            tx = x;
            ty = y;
        }
        for (int k = 0; k < 4; ++k) {
            int dx = dirs[k], dy = dirs[k + 1];
            int nx = x + dx, ny = y + dy;
            if (nx >= 0 && nx < m && ny >= 0 && ny < n && g[nx][ny] == -1 && master.canMove(s[k])) {
                g[nx][ny] = master.move(s[k]);
                dfs(nx, ny);
                master.move(s[(k + 2) % 4]);
            }
        }
    }
}
```

#### C++

```cpp
/**
 * // This is the GridMaster's API interface.
 * // You should not implement it, or speculate about its implementation
 * class GridMaster {
 *   public:
 *     bool canMove(char direction);
 *     int move(char direction);
 *     boolean isTarget();
 * };
 */

class Solution {
public:
    int findShortestPath(GridMaster& master) {
        const int m = 200, n = 200;
        const int sx = 100, sy = 100;
        const int INF = INT_MAX / 2;
        int dirs[5] = {-1, 0, 1, 0, -1};
        char s[4] = {'U', 'R', 'D', 'L'};

        vector<vector<int>> g(m, vector<int>(n, -1));
        pair<int, int> target = {-1, -1};

        auto dfs = [&](this auto& dfs, int x, int y) -> void {
            if (master.isTarget()) {
                target = {x, y};
            }
            for (int k = 0; k < 4; ++k) {
                int dx = dirs[k], dy = dirs[k + 1];
                int nx = x + dx, ny = y + dy;
                if (0 <= nx && nx < m && 0 <= ny && ny < n && g[nx][ny] == -1 && master.canMove(s[k])) {
                    g[nx][ny] = master.move(s[k]);
                    dfs(nx, ny);
                    master.move(s[(k + 2) % 4]);
                }
            }
        };

        g[sx][sy] = 0;
        dfs(sx, sy);

        if (target.first == -1 && target.second == -1) {
            return -1;
        }

        vector<vector<int>> dist(m, vector<int>(n, INF));
        dist[sx][sy] = 0;

        using Node = tuple<int, int, int>;
        priority_queue<Node, vector<Node>, greater<Node>> pq;
        pq.emplace(0, sx, sy);

        while (!pq.empty()) {
            auto [w, x, y] = pq.top();
            pq.pop();
            if (x == target.first && y == target.second) {
                return w;
            }
            if (w > dist[x][y]) {
                continue;
            }

            for (int k = 0; k < 4; ++k) {
                int nx = x + dirs[k], ny = y + dirs[k + 1];
                if (0 <= nx && nx < m && 0 <= ny && ny < n && g[nx][ny] != -1) {
                    int nd = w + g[nx][ny];
                    if (nd < dist[nx][ny]) {
                        dist[nx][ny] = nd;
                        pq.emplace(nd, nx, ny);
                    }
                }
            }
        }

        return -1;
    }
};
```

#### JavaScript

```js
/**
 * // This is the GridMaster's API interface.
 * // You should not implement it, or speculate about its implementation
 * function GridMaster() {
 *
 *     @param {character} direction
 *     @return {boolean}
 *     this.canMove = function(direction) {
 *         ...
 *     };
 *     @param {character} direction
 *     @return {integer}
 *     this.move = function(direction) {
 *         ...
 *     };
 *     @return {boolean}
 *     this.isTarget = function() {
 *         ...
 *     };
 * };
 */

/**
 * @param {GridMaster} master
 * @return {integer}
 */
var findShortestPath = function (master) {
    const [m, n] = [200, 200];
    const [sx, sy] = [100, 100];
    const inf = Number.MAX_SAFE_INTEGER;
    const dirs = [-1, 0, 1, 0, -1];
    const s = ['U', 'R', 'D', 'L'];
    const g = Array.from({ length: m }, () => Array(n).fill(-1));
    let target = [-1, -1];
    const dfs = (x, y) => {
        if (master.isTarget()) {
            target = [x, y];
        }
        for (let k = 0; k < 4; ++k) {
            const dx = dirs[k],
                dy = dirs[k + 1];
            const nx = x + dx,
                ny = y + dy;
            if (
                0 <= nx &&
                nx < m &&
                0 <= ny &&
                ny < n &&
                g[nx][ny] === -1 &&
                master.canMove(s[k])
            ) {
                g[nx][ny] = master.move(s[k]);
                dfs(nx, ny);
                master.move(s[(k + 2) % 4]);
            }
        }
    };
    g[sx][sy] = 0;
    dfs(sx, sy);
    if (target[0] === -1 && target[1] === -1) {
        return -1;
    }
    const dist = Array.from({ length: m }, () => Array(n).fill(inf));
    dist[sx][sy] = 0;
    const pq = new MinPriorityQueue(node => node[0]);
    pq.enqueue([0, sx, sy]);
    while (!pq.isEmpty()) {
        const [w, x, y] = pq.dequeue();
        if (x === target[0] && y === target[1]) {
            return w;
        }
        if (w > dist[x][y]) {
            continue;
        }
        for (let k = 0; k < 4; ++k) {
            const nx = x + dirs[k],
                ny = y + dirs[k + 1];
            if (0 <= nx && nx < m && 0 <= ny && ny < n && g[nx][ny] !== -1) {
                const nd = w + g[nx][ny];
                if (nd < dist[nx][ny]) {
                    dist[nx][ny] = nd;
                    pq.enqueue([nd, nx, ny]);
                }
            }
        }
    }
    return -1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
