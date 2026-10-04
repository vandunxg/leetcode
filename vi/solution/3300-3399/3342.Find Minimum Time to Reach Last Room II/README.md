---
comments: true
difficulty: Medium
rating: 1861
source: Weekly Contest 422 Q3
tags:
    - Graph
    - Array
    - Matrix
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3342. Find Minimum Time to Reach Last Room II](https://leetcode.com/problems/find-minimum-time-to-reach-last-room-ii)

[中文文档](/solution/3300-3399/3342.Find%20Minimum%20Time%20to%20Reach%20Last%20Room%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có một dungeon gồm <code>n x m</code> phòng được sắp xếp thành một lưới.</p>

<p>Cho một mảng hai chiều <code>moveTime</code> có kích thước <code>n x m</code>, trong đó <code>moveTime[i][j]</code> biểu thị thời điểm <strong>sớm nhất</strong> tính bằng giây mà bạn có thể <strong>bắt đầu di chuyển</strong> đến phòng đó. Bạn bắt đầu từ phòng <code>(0, 0)</code> tại thời điểm <code>t = 0</code> và có thể di chuyển đến phòng <strong>kề nhau</strong>. Việc di chuyển giữa các phòng <strong>kề nhau</strong> mất một giây cho một lần di chuyển và hai giây cho lần tiếp theo, <strong>luân phiên</strong> giữa hai khoảng thời gian này.</p>

<p>Trả về thời gian <strong>nhỏ nhất</strong> để đến phòng <code>(n - 1, m - 1)</code>.</p>

<p>Hai phòng <strong>kề nhau</strong> nếu chúng có chung một cạnh, theo chiều <em>ngang</em> hoặc <em>dọc</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">moveTime = [[0,4],[4,4]]</span></p>

<p><strong>Đầu ra:</strong> 7</p>

<p><strong>Giải thích:</strong></p>

<p>Thời gian nhỏ nhất cần thiết là 7 giây.</p>

<ul>
    <li>Tại thời điểm <code>t == 4</code>, di chuyển từ phòng <code>(0, 0)</code> đến phòng <code>(1, 0)</code> trong một giây.</li>
    <li>Tại thời điểm <code>t == 5</code>, di chuyển từ phòng <code>(1, 0)</code> đến phòng <code>(1, 1)</code> trong hai giây.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">moveTime = [[0,0,0,0],[0,0,0,0]]</span></p>

<p><strong>Đầu ra:</strong> 6</p>

<p><strong>Giải thích:</strong></p>

<p>Thời gian nhỏ nhất cần thiết là 6 giây.</p>

<ul>
    <li>Tại thời điểm <code>t == 0</code>, di chuyển từ phòng <code>(0, 0)</code> đến phòng <code>(1, 0)</code> trong một giây.</li>
    <li>Tại thời điểm <code>t == 1</code>, di chuyển từ phòng <code>(1, 0)</code> đến phòng <code>(1, 1)</code> trong hai giây.</li>
    <li>Tại thời điểm <code>t == 3</code>, di chuyển từ phòng <code>(1, 1)</code> đến phòng <code>(1, 2)</code> trong một giây.</li>
    <li>Tại thời điểm <code>t == 4</code>, di chuyển từ phòng <code>(1, 2)</code> đến phòng <code>(1, 3)</code> trong hai giây.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">moveTime = [[0,1],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> 4</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n == moveTime.length &lt;= 750</code></li>
    <li><code>2 &lt;= m == moveTime[i].length &lt;= 750</code></li>
    <li><code>0 &lt;= moveTime[i][j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijkstra

<!-- thinking:start -->

> **Tư duy**
>
> So với phần I, các bước lẻ và chẵn có chi phí lần lượt là $1$ và $2$, đồng thời lưới cũng lớn hơn. Mô hình đường đi ngắn nhất không thay đổi; trọng số cạnh là $\max(\textit{moveTime}[x][y],d)+(i+j)\bmod 2+1$.
>
> $(i+j)\bmod 2$ là tính chẵn lẻ của bước tiếp theo sau khi đến $(i,j)$: từ $(0,0)$, bước đầu tiên có chi phí $1$ và bước tiếp theo có chi phí $2$.
>
> Dijkstra vẫn thực hiện relaxation theo thời gian; lần đầu lấy đích ra khỏi hàng đợi chính là đáp án.

<!-- thinking:end -->

Ta định nghĩa một mảng hai chiều $\textit{dist}$, trong đó $\textit{dist}[i][j]$ biểu thị thời gian nhỏ nhất cần để đi từ điểm bắt đầu đến phòng $(i, j)$. Ban đầu, ta đặt tất cả phần tử trong mảng $\textit{dist}$ bằng vô cực, sau đó đặt giá trị $\textit{dist}$ của điểm bắt đầu $(0, 0)$ bằng $0$.

Ta sử dụng một priority queue $\textit{pq}$ để lưu mỗi trạng thái, trong đó mỗi trạng thái gồm ba giá trị $(d, i, j)$, biểu thị thời gian $d$ cần để đi từ điểm bắt đầu đến phòng $(i, j)$. Ban đầu, ta thêm điểm bắt đầu $(0, 0, 0)$ vào $\textit{pq}$.

Ở mỗi bước lặp, ta lấy phần tử đầu $(d, i, j)$ từ $\textit{pq}$. Nếu $(i, j)$ là đích, ta trả về $d$. Nếu $d$ lớn hơn $\textit{dist}[i][j]$, ta bỏ qua trạng thái này. Nếu không, ta duyệt bốn vị trí kề nhau $(x, y)$ của $(i, j)$. Nếu $(x, y)$ nằm trong lưới, ta tính thời điểm hoàn thành $t$ khi di chuyển từ $(i, j)$ đến $(x, y)$ theo công thức $t = \max(\textit{moveTime}[x][y], \textit{dist}[i][j]) + (i + j) \bmod 2 + 1$. Nếu $t$ nhỏ hơn $\textit{dist}[x][y]$, ta cập nhật giá trị của $\textit{dist}[x][y]$ và thêm $(t, x, y)$ vào $\textit{pq}$.

Độ phức tạp thời gian là $O(n \times m \times \log (n \times m))$, và độ phức tạp không gian là $O(n \times m)$. Trong đó, $n$ và $m$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTimeToReach(self, moveTime: List[List[int]]) -> int:
        n, m = len(moveTime), len(moveTime[0])
        dist = [[inf] * m for _ in range(n)]
        dist[0][0] = 0
        pq = [(0, 0, 0)]
        dirs = (-1, 0, 1, 0, -1)
        while 1:
            d, i, j = heappop(pq)
            if i == n - 1 and j == m - 1:
                return d
            if d > dist[i][j]:
                continue
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < n and 0 <= y < m:
                    t = max(moveTime[x][y], dist[i][j]) + (i + j) % 2 + 1
                    if dist[x][y] > t:
                        dist[x][y] = t
                        heappush(pq, (t, x, y))
```

#### Java

```java
class Solution {
    public int minTimeToReach(int[][] moveTime) {
        int n = moveTime.length;
        int m = moveTime[0].length;
        int[][] dist = new int[n][m];
        for (var row : dist) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        dist[0][0] = 0;

        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {0, 0, 0});
        int[] dirs = {-1, 0, 1, 0, -1};
        while (true) {
            int[] p = pq.poll();
            int d = p[0], i = p[1], j = p[2];

            if (i == n - 1 && j == m - 1) {
                return d;
            }
            if (d > dist[i][j]) {
                continue;
            }

            for (int k = 0; k < 4; k++) {
                int x = i + dirs[k];
                int y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < m) {
                    int t = Math.max(moveTime[x][y], dist[i][j]) + (i + j) % 2 + 1;
                    if (dist[x][y] > t) {
                        dist[x][y] = t;
                        pq.offer(new int[] {t, x, y});
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
    int minTimeToReach(vector<vector<int>>& moveTime) {
        int n = moveTime.size();
        int m = moveTime[0].size();
        vector<vector<int>> dist(n, vector<int>(m, INT_MAX));
        dist[0][0] = 0;
        priority_queue<array<int, 3>, vector<array<int, 3>>, greater<>> pq;
        pq.push({0, 0, 0});
        int dirs[5] = {-1, 0, 1, 0, -1};

        while (1) {
            auto [d, i, j] = pq.top();
            pq.pop();

            if (i == n - 1 && j == m - 1) {
                return d;
            }
            if (d > dist[i][j]) {
                continue;
            }

            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k];
                int y = j + dirs[k + 1];

                if (x >= 0 && x < n && y >= 0 && y < m) {
                    int t = max(moveTime[x][y], dist[i][j]) + (i + j) % 2 + 1;
                    if (dist[x][y] > t) {
                        dist[x][y] = t;
                        pq.push({t, x, y});
                    }
                }
            }
        }
    }
};
```

#### Go

```go
func minTimeToReach(moveTime [][]int) int {
    n, m := len(moveTime), len(moveTime[0])
    dist := make([][]int, n)
    for i := range dist {
        dist[i] = make([]int, m)
        for j := range dist[i] {
            dist[i][j] = math.MaxInt32
        }
    }
    dist[0][0] = 0

    pq := &hp{}
    heap.Init(pq)
    heap.Push(pq, tuple{0, 0, 0})

    dirs := []int{-1, 0, 1, 0, -1}
    for {
        p := heap.Pop(pq).(tuple)
        d, i, j := p.dis, p.x, p.y

        if i == n-1 && j == m-1 {
            return d
        }
        if d > dist[i][j] {
            continue
        }

        for k := 0; k < 4; k++ {
            x, y := i+dirs[k], j+dirs[k+1]
            if x >= 0 && x < n && y >= 0 && y < m {
                t := max(moveTime[x][y], dist[i][j]) + (i+j)%2 + 1
                if dist[x][y] > t {
                    dist[x][y] = t
                    heap.Push(pq, tuple{t, x, y})
                }
            }
        }
    }
}

type tuple struct{ dis, x, y int }
type hp []tuple

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].dis < h[j].dis }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(tuple)) }
func (h *hp) Pop() (v any)      { a := *h; *h, v = a[:len(a)-1], a[len(a)-1]; return }
```

#### TypeScript

```ts
function minTimeToReach(moveTime: number[][]): number {
    const n = moveTime.length;
    const m = moveTime[0].length;
    const dist = Array.from({ length: n }, () => Array(m).fill(Infinity));
    dist[0][0] = 0;
    type Node = [number, number, number];
    const pq = new PriorityQueue<Node>((a, b) => a[0] - b[0]);
    pq.enqueue([0, 0, 0]);
    const dirs = [-1, 0, 1, 0, -1];
    while (!pq.isEmpty()) {
        const [d, i, j] = pq.dequeue();
        if (d > dist[i][j]) continue;
        if (i === n - 1 && j === m - 1) return d;
        for (let k = 0; k < 4; k++) {
            const x = i + dirs[k];
            const y = j + dirs[k + 1];
            if (x >= 0 && x < n && y >= 0 && y < m) {
                const t = Math.max(moveTime[x][y], d) + ((i + j) % 2) + 1;
                if (t < dist[x][y]) {
                    dist[x][y] = t;
                    pq.enqueue([t, x, y]);
                }
            }
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
