---
comments: true
difficulty: Hard
rating: 2381
source: Weekly Contest 334 Q4
tags:
    - Breadth-First Search
    - Graph
    - Array
    - Matrix
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2577. Minimum Time to Visit a Cell In a Grid](https://leetcode.com/problems/minimum-time-to-visit-a-cell-in-a-grid)

[中文文档](/solution/2500-2599/2577.Minimum%20Time%20to%20Visit%20a%20Cell%20In%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <code>m x n</code> <code>grid</code> gồm các số nguyên <b>không âm</b>, trong đó <code>grid[row][col]</code> biểu thị <strong>thời điểm sớm nhất</strong> có thể đi vào ô <code>(row, col)</code>, nghĩa là bạn chỉ có thể đi vào ô <code>(row, col)</code> khi thời điểm bạn đến đó lớn hơn hoặc bằng <code>grid[row][col]</code>.</p>

<p>Bạn đang đứng ở ô <strong>góc trên bên trái</strong> của ma trận tại giây thứ <code>0<sup>th</sup></code>, và phải di chuyển đến <strong>một</strong> ô kề theo một trong bốn hướng: lên, xuống, trái và phải. Mỗi lần di chuyển mất 1 giây.</p>

<p>Hãy trả về <em><strong>thời điểm nhỏ nhất</strong> cần thiết để bạn có thể đi vào ô dưới cùng bên phải của ma trận</em>. Nếu không thể đi vào ô dưới cùng bên phải, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2577.Minimum%20Time%20to%20Visit%20a%20Cell%20In%20a%20Grid/images/yetgriddrawio-8.png" /></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,1,3,2],[5,1,2,5],[4,3,8,6]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Một trong những đường đi ta có thể chọn là:
- tại t = 0, ta đang ở ô (0,0).
- tại t = 1, ta di chuyển đến ô (0,1). Điều này khả thi vì grid[0][1] &lt;= 1.
- tại t = 2, ta di chuyển đến ô (1,1). Điều này khả thi vì grid[1][1] &lt;= 2.
- tại t = 3, ta di chuyển đến ô (1,2). Điều này khả thi vì grid[1][2] &lt;= 3.
- tại t = 4, ta di chuyển đến ô (1,1). Điều này khả thi vì grid[1][1] &lt;= 4.
- tại t = 5, ta di chuyển đến ô (1,2). Điều này khả thi vì grid[1][2] &lt;= 5.
- tại t = 6, ta di chuyển đến ô (1,3). Điều này khả thi vì grid[1][3] &lt;= 6.
- tại t = 7, ta di chuyển đến ô (2,3). Điều này khả thi vì grid[2][3] &lt;= 7.
Thời điểm cuối cùng là 7. Có thể chứng minh đây là thời điểm nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2577.Minimum%20Time%20to%20Visit%20a%20Cell%20In%20a%20Grid/images/yetgriddrawio-9.png" style="width: 151px; height: 151px;" /></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,2,4],[3,2,1],[1,0,4]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có đường đi từ ô trên cùng bên trái đến ô dưới cùng bên phải.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 1000</code></li>
	<li><code>4 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>grid[0][0] == 0</code></li>
</ul>

<p>&nbsp;</p>
<style type="text/css">.spoilerbutton {display:block; border:dashed; padding: 0px 0px; margin:10px 0px; font-size:150%; font-weight: bold; color:#000000; background-color:cyan; outline:0;
}
.spoiler {overflow:hidden;}
.spoiler > div {-webkit-transition: all 0s ease;-moz-transition: margin 0s ease;-o-transition: all 0s ease;transition: margin 0s ease;}
.spoilerbutton[value="Show Message"] + .spoiler > div {margin-top:-500%;}
.spoilerbutton[value="Hide Message"] + .spoiler {padding:5px;}
</style>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đường đi ngắn nhất + Hàng đợi ưu tiên (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Một ô chỉ có thể được đi vào khi thời điểm đến ít nhất bằng giá trị của ô; mỗi bước đi mất $1$, và ta có thể đi qua đi lại để chờ. Nếu cả hai ô kề với ô bắt đầu đều có giá trị lớn hơn $1$, bước đi đầu tiên là bất khả thi.
>
> Nếu không, ta luôn có thể điều chỉnh tính chẵn lẻ bằng cách đi qua đi lại. Dijkstra sử dụng min-heap lưu các thời điểm đến: nếu $t+1$ đã đủ lớn thì đi ngay; nếu chưa, chờ đến $grid[x][y]$, cộng thêm một giây nếu thời điểm đó có tính chẵn lẻ không phù hợp với $t+1$.

<!-- thinking:end -->

Ta nhận thấy rằng nếu không thể di chuyển từ ô $(0, 0)$, tức là $grid[0][1] > 1$ và $grid[1][0] > 1$, thì sau đó ta cũng không thể di chuyển khỏi ô $(0, 0)$, nên cần trả về $-1$. Trong các trường hợp khác, ta có thể di chuyển.

Tiếp theo, ta định nghĩa $dist[i][j]$ là thời điểm sớm nhất đến được ô $(i, j)$. Ban đầu, $dist[0][0] = 0$, còn $dist$ của các vị trí khác đều được khởi tạo là $\infty$.

Ta sử dụng một hàng đợi ưu tiên (min heap) để lưu các ô hiện có thể di chuyển đến. Các phần tử trong hàng đợi ưu tiên có dạng $(dist[i][j], i, j)$, trong đó $(dist[i][j], i, j)$ biểu thị thời điểm sớm nhất đến được ô $(i, j)$.

Mỗi lần, ta lấy ra ô $(t, i, j)$ có thời điểm đến sớm nhất từ hàng đợi ưu tiên. Nếu $(i, j)$ là $(m - 1, n - 1)$, ta trả về $t$. Nếu không, ta duyệt bốn ô kề $(x, y)$ của $(i, j)$ theo các hướng lên, xuống, trái và phải. Nếu $t + 1 < grid[x][y]$, thì thời điểm di chuyển đến $(x, y)$ là $nt = grid[x][y] + (grid[x][y] - (t + 1)) \bmod 2$. Tại thời điểm này, ta có thể liên tục di chuyển để thời gian đạt ít nhất $grid[x][y]$, tùy thuộc vào tính chẵn lẻ của khoảng cách giữa $t + 1$ và $grid[x][y]$. Ngược lại, thời điểm di chuyển đến $(x, y)$ là $nt = t + 1$. Nếu $nt < dist[x][y]$, ta cập nhật $dist[x][y] = nt$ và thêm $(nt, x, y)$ vào hàng đợi ưu tiên.

Độ phức tạp thời gian là $O(m \times n \times \log (m \times n))$, và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, grid: List[List[int]]) -> int:
        if grid[0][1] > 1 and grid[1][0] > 1:
            return -1
        m, n = len(grid), len(grid[0])
        dist = [[inf] * n for _ in range(m)]
        dist[0][0] = 0
        q = [(0, 0, 0)]
        dirs = (-1, 0, 1, 0, -1)
        while 1:
            t, i, j = heappop(q)
            if i == m - 1 and j == n - 1:
                return t
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n:
                    nt = t + 1
                    if nt < grid[x][y]:
                        nt = grid[x][y] + (grid[x][y] - nt) % 2
                    if nt < dist[x][y]:
                        dist[x][y] = nt
                        heappush(q, (nt, x, y))
```

#### Java

```java
class Solution {
    public int minimumTime(int[][] grid) {
        if (grid[0][1] > 1 && grid[1][0] > 1) {
            return -1;
        }
        int m = grid.length, n = grid[0].length;
        int[][] dist = new int[m][n];
        for (var e : dist) {
            Arrays.fill(e, 1 << 30);
        }
        dist[0][0] = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {0, 0, 0});
        int[] dirs = {-1, 0, 1, 0, -1};
        while (true) {
            var p = pq.poll();
            int i = p[1], j = p[2];
            if (i == m - 1 && j == n - 1) {
                return p[0];
            }
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n) {
                    int nt = p[0] + 1;
                    if (nt < grid[x][y]) {
                        nt = grid[x][y] + (grid[x][y] - nt) % 2;
                    }
                    if (nt < dist[x][y]) {
                        dist[x][y] = nt;
                        pq.offer(new int[] {nt, x, y});
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
    int minimumTime(vector<vector<int>>& grid) {
        if (grid[0][1] > 1 && grid[1][0] > 1) {
            return -1;
        }
        int m = grid.size(), n = grid[0].size();
        int dist[m][n];
        memset(dist, 0x3f, sizeof dist);
        dist[0][0] = 0;
        using tii = tuple<int, int, int>;
        priority_queue<tii, vector<tii>, greater<tii>> pq;
        pq.emplace(0, 0, 0);
        int dirs[5] = {-1, 0, 1, 0, -1};
        while (1) {
            auto [t, i, j] = pq.top();
            pq.pop();
            if (i == m - 1 && j == n - 1) {
                return t;
            }
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n) {
                    int nt = t + 1;
                    if (nt < grid[x][y]) {
                        nt = grid[x][y] + (grid[x][y] - nt) % 2;
                    }
                    if (nt < dist[x][y]) {
                        dist[x][y] = nt;
                        pq.emplace(nt, x, y);
                    }
                }
            }
        }
    }
};
```

#### Go

```go
func minimumTime(grid [][]int) int {
	if grid[0][1] > 1 && grid[1][0] > 1 {
		return -1
	}
	m, n := len(grid), len(grid[0])
	dist := make([][]int, m)
	for i := range dist {
		dist[i] = make([]int, n)
		for j := range dist[i] {
			dist[i][j] = 1 << 30
		}
	}
	dist[0][0] = 0
	pq := hp{}
	heap.Push(&pq, tuple{0, 0, 0})
	dirs := [5]int{-1, 0, 1, 0, -1}
	for {
		p := heap.Pop(&pq).(tuple)
		i, j := p.i, p.j
		if i == m-1 && j == n-1 {
			return p.t
		}
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n {
				nt := p.t + 1
				if nt < grid[x][y] {
					nt = grid[x][y] + (grid[x][y]-nt)%2
				}
				if nt < dist[x][y] {
					dist[x][y] = nt
					heap.Push(&pq, tuple{nt, x, y})
				}
			}
		}
	}
}

type tuple struct{ t, i, j int }
type hp []tuple

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].t < h[j].t }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(tuple)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function minimumTime(grid: number[][]): number {
    if (grid[0][1] > 1 && grid[1][0] > 1) return -1;

    const [m, n] = [grid.length, grid[0].length];
    const DIRS = [-1, 0, 1, 0, -1];
    const q = new PriorityQueue<number[]>((a, b) => a[0] - b[0]);
    const dist: number[][] = Array.from({ length: m }, () =>
        new Array(n).fill(Number.POSITIVE_INFINITY),
    );
    dist[0][0] = 0;
    q.enqueue([0, 0, 0]);

    while (true) {
        const [t, i, j] = q.dequeue();
        if (i === m - 1 && j === n - 1) return t;

        for (let k = 0; k < 4; k++) {
            const [x, y] = [i + DIRS[k], j + DIRS[k + 1]];
            if (x < 0 || x >= m || y < 0 || y >= n) continue;

            let nt = t + 1;
            if (nt < grid[x][y]) {
                nt = grid[x][y] + ((grid[x][y] - nt) % 2);
            }
            if (nt < dist[x][y]) {
                dist[x][y] = nt;
                q.enqueue([nt, x, y]);
            }
        }
    }
}
```

#### JavaScript

```js
function minimumTime(grid) {
    if (grid[0][1] > 1 && grid[1][0] > 1) return -1;

    const [m, n] = [grid.length, grid[0].length];
    const DIRS = [-1, 0, 1, 0, -1];
    const q = new PriorityQueue((a, b) => a[0] - b[0]);
    const dist = Array.from({ length: m }, () => new Array(n).fill(Number.POSITIVE_INFINITY));
    dist[0][0] = 0;
    q.enqueue([0, 0, 0]);

    while (true) {
        const [t, i, j] = q.dequeue();
        if (i === m - 1 && j === n - 1) return t;

        for (let k = 0; k < 4; k++) {
            const [x, y] = [i + DIRS[k], j + DIRS[k + 1]];
            if (x < 0 || x >= m || y < 0 || y >= n) continue;

            let nt = t + 1;
            if (nt < grid[x][y]) {
                nt = grid[x][y] + ((grid[x][y] - nt) % 2);
            }
            if (nt < dist[x][y]) {
                dist[x][y] = nt;
                q.enqueue([nt, x, y]);
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
