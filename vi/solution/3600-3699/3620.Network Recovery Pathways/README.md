---
comments: true
difficulty: Hard
rating: 1998
source: Biweekly Contest 161 Q3
tags:
    - Graph
    - Topological Sort
    - Array
    - Binary Search
    - Dynamic Programming
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3620. Network Recovery Pathways](https://leetcode.com/problems/network-recovery-pathways)

[中文文档](/solution/3600-3699/3620.Network%20Recovery%20Pathways/README.md)

## Mô tả

<!-- description:start -->

<p data-end="502" data-start="75">Bạn được cung cấp một đồ thị có hướng không chu trình gồm <code>n</code> node được đánh số từ 0 đến <code>n &minus; 1</code>. Đồ thị được biểu diễn bằng một mảng 2D <code data-end="201" data-start="194">edges</code> có độ dài<font face="monospace"> <code>m</code></font>, trong đó <code data-end="255" data-start="227">edges[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>]</code> biểu thị một liên lạc một chiều từ node <code data-end="304" data-start="300">u<sub>i</sub></code> đến node <code data-end="317" data-start="313">v<sub>i</sub></code> với chi phí khôi phục là <code data-end="349" data-start="342">cost<sub>i</sub></code>.</p>

<p data-end="502" data-start="75">Một số node có thể đang offline. Bạn được cung cấp một mảng boolean <code data-end="416" data-start="408">online</code>, trong đó <code data-end="441" data-start="423">online[i] = true</code> nghĩa là node <code data-end="456" data-start="453">i</code> đang online. Node 0 và <code>n &minus; 1</code> luôn online.</p>

<p data-end="547" data-start="504">Một đường đi từ 0 đến <code>n &minus; 1</code> là <strong data-end="541" data-start="532">hợp lệ</strong> nếu:</p>

<ul>
    <li>Tất cả node trung gian trên đường đi đều online.</li>
    <li data-end="676" data-start="605">Tổng chi phí khôi phục của tất cả các cạnh trên đường đi không vượt quá <code>k</code>.</li>
</ul>

<p data-end="771" data-start="653">Với mỗi đường đi hợp lệ, định nghĩa <strong data-end="694" data-start="685">score</strong> của đường đi là chi phí cạnh nhỏ nhất trên đường đi đó.</p>

<p data-end="913" data-start="847">Trả về <strong>score lớn nhất</strong> của đường đi (tức là <strong>chi phí cạnh nhỏ nhất</strong> lớn nhất) trong tất cả các đường đi hợp lệ. Nếu không tồn tại đường đi hợp lệ, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1,5],[1,3,10],[0,2,3],[2,3,4]], online = [true,true,true,true], k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3620.Network%20Recovery%20Pathways/images/graph-10.png" style="width: 239px; height: 267px;" /></p>

<ul data-end="551" data-start="146">
    <li data-end="462" data-start="146">
    <p data-end="206" data-start="148">Đồ thị có hai tuyến đường khả dĩ từ node 0 đến node 3:</p>

    <ol data-end="462" data-start="209">
    <li data-end="315" data-start="209">
    <p data-end="228" data-start="212">Đường đi <code>0 &rarr; 1 &rarr; 3</code></p>

    <ul data-end="315" data-start="234">
    <li data-end="315" data-start="234">
    <p data-end="315" data-start="236">Tổng chi phí = <code>5 + 10 = 15</code>, vượt quá k (<code>15 &gt; 10</code>), nên đường đi này không hợp lệ.</p>
    </li>
    </ul>
    </li>
    <li data-end="462" data-start="318">
    <p data-end="337" data-start="321">Đường đi <code>0 &rarr; 2 &rarr; 3</code></p>

    <ul data-end="462" data-start="343">
        <li data-end="397" data-start="343">
        <p data-end="397" data-start="345">Tổng chi phí = <code>3 + 4 = 7 &lt;= k</code>, nên đường đi này hợp lệ.</p>
        </li>
        <li data-end="462" data-start="403">
        <p data-end="462" data-start="405">Chi phí cạnh nhỏ nhất trên đường đi này là <code>min(3, 4) = 3</code>.</p>
        </li>
    </ul>
    </li>
    </ol>
    </li>
     <li data-end="551" data-start="463">
     <p data-end="551" data-start="465">Không còn đường đi hợp lệ nào khác. Vì vậy, giá trị lớn nhất trong các score của đường đi là 3.</p>
     </li>

 </ul>
 </div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1,7],[1,4,5],[0,2,6],[2,3,6],[3,4,2],[2,4,6]], online = [true,true,true,false,true], k = 12</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3620.Network%20Recovery%20Pathways/images/graph-11.png" style="width: 343px; height: 194px;" /></p>

<ul>
    <li data-end="790" data-start="726">
    <p data-end="790" data-start="728">Node 3 đang offline, nên mọi đường đi qua node 3 đều không hợp lệ.</p>
    </li>
    <li data-end="1231" data-start="791">
    <p data-end="837" data-start="793">Xét các tuyến đường còn lại từ node 0 đến node 4:</p>

    <ol data-end="1231" data-start="840">
    <li data-end="985" data-start="840">
    <p data-end="859" data-start="843">Đường đi <code>0 &rarr; 1 &rarr; 4</code></p>

    <ul data-end="985" data-start="865">
        <li data-end="920" data-start="865">
        <p data-end="920" data-start="867">Tổng chi phí = <code>7 + 5 = 12 &lt;= k</code>, nên đường đi này hợp lệ.</p>
        </li>
        <li data-end="985" data-start="926">
        <p data-end="985" data-start="928">Chi phí cạnh nhỏ nhất trên đường đi này là <code>min(7, 5) = 5</code>.</p>
        </li>
    </ul>
    </li>
    <li data-end="1083" data-start="988">
    <p data-end="1011" data-start="991">Đường đi <code>0 &rarr; 2 &rarr; 3 &rarr; 4</code></p>

    <ul data-end="1083" data-start="1017">
        <li data-end="1083" data-start="1017">
        <p data-end="1083" data-start="1019">Node 3 đang offline, nên đường đi này không hợp lệ bất kể chi phí là bao nhiêu.</p>
        </li>
    </ul>
    </li>
    <li data-end="1231" data-start="1086">
    <p data-end="1105" data-start="1089">Đường đi <code>0 &rarr; 2 &rarr; 4</code></p>

    <ul data-end="1231" data-start="1111">
        <li data-end="1166" data-start="1111">
        <p data-end="1166" data-start="1113">Tổng chi phí = <code>6 + 6 = 12 &lt;= k</code>, nên đường đi này hợp lệ.</p>
        </li>
        <li data-end="1231" data-start="1172">
        <p data-end="1231" data-start="1174">Chi phí cạnh nhỏ nhất trên đường đi này là <code>min(6, 6) = 6</code>.</p>
        </li>
    </ul>
    </li>
    </ol>
    </li>
     <li data-end="1314" data-is-last-node="" data-start="1232">
     <p data-end="1314" data-is-last-node="" data-start="1234">Hai đường đi hợp lệ có score lần lượt là 5 và 6. Do đó, đáp án là 6.</p>
     </li>

 </ul>
 </div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="42" data-start="20"><code data-end="40" data-start="20">n == online.length</code></li>
    <li data-end="63" data-start="45"><code data-end="61" data-start="45">2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
    <li data-end="102" data-start="66"><code data-end="100" data-start="66">0 &lt;= m == edges.length &lt;= </code><code>min(10<sup>5</sup>, n * (n - 1) / 2)</code></li>
    <li data-end="102" data-start="66"><code data-end="127" data-start="105">edges[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>]</code></li>
    <li data-end="151" data-start="132"><code data-end="149" data-start="132">0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li data-end="166" data-start="154"><code data-end="164" data-start="154">u<sub>i</sub> != v<sub>i</sub></code></li>
    <li data-end="191" data-start="169"><code data-end="189" data-start="169">0 &lt;= cost<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
    <li data-end="213" data-start="194"><code data-end="211" data-start="194">0 &lt;= k &lt;= 5 * 10<sup>13</sup></code></li>
    <li data-end="309" data-start="216"><code data-end="227" data-start="216">online[i]</code> là <code data-end="244" data-is-only-node="" data-start="238">true</code> hoặc <code data-end="255" data-start="248">false</code>, đồng thời cả <code data-end="277" data-start="266">online[0]</code> và <code data-end="295" data-start="282">online[n &minus; 1]</code> đều là <code data-end="306" data-start="300">true</code>.</li>
    <li data-end="362" data-is-last-node="" data-start="312">Đồ thị đã cho là một đồ thị có hướng không chu trình.</li>
 </ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Dijkstra tối ưu bằng heap

<!-- thinking:start -->

> **Tư duy**
>
> Score của một đường đi là cạnh nhẹ nhất trên đường đi đó. Ta cần tối đa hóa giá trị nhỏ nhất này dưới giới hạn tổng chi phí $k$ và chỉ được đi qua các node online. Không thể liệt kê tất cả các đường đi.
>
> Ngưỡng lớn hơn sẽ giữ lại ít cạnh hơn và tính khả thi có tính đơn điệu, nên ta dùng tìm kiếm nhị phân trên trọng số cạnh nhỏ nhất. Với $\textit{mid}$, loại bỏ các cạnh nhẹ hơn $\textit{mid}$ rồi chạy Dijkstra từ $0$ đến $n-1$ bằng heap, sau đó so sánh khoảng cách với $k$.
>
> Bỏ qua mọi cạnh có endpoint offline. Nếu ngay cả ứng viên nhỏ nhất cũng không hợp lệ, trả về $-1$.

<!-- thinking:end -->

Score của đường đi được định nghĩa là chi phí cạnh nhỏ nhất trên đường đi đó. Ta cần tìm score lớn nhất trong tất cả các đường đi hợp lệ.

Với một trọng số cạnh nhỏ nhất ứng viên $mid$, ta chỉ giữ lại các cạnh có chi phí ít nhất là $mid$, sau đó kiểm tra xem có tồn tại đường đi từ node $0$ đến node $n - 1$ với tổng chi phí không vượt quá $k$ hay không. Việc này tương đương với chạy Dijkstra tối ưu bằng heap trên đồ thị đã lọc.

Khi $mid$ tăng, số cạnh còn lại giảm và điều kiện khả thi trở nên khó thỏa mãn hơn, nên ta có thể dùng tìm kiếm nhị phân trên $mid$. Trước đó, ta loại bỏ các cạnh kề với node offline, đồng thời đặt $l$ và $r$ lần lượt là chi phí cạnh nhỏ nhất và lớn nhất. Nếu $check(l)$ đúng, trả về $l$; nếu không, trả về $-1$.

Độ phức tạp thời gian là $O((n + m) \log n \log W)$, và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh, còn $W$ là chi phí cạnh lớn nhất.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxPathScore(
        self, edges: List[List[int]], online: List[bool], k: int
    ) -> int:
        def check(mid: int) -> int:
            dist = [inf] * n
            dist[0] = 0
            pq = [(0, 0)]
            while pq:
                d, u = heappop(pq)
                if d > k:
                    return False
                if u == n - 1:
                    return True
                if dist[u] < d:
                    continue
                for v, w in g[u]:
                    if w < mid:
                        continue
                    if dist[u] + w < dist[v]:
                        dist[v] = dist[u] + w
                        heappush(pq, (dist[v], v))
            return False

        n = len(online)
        g = [[] for _ in range(n)]
        l, r = inf, 0
        for (
            u,
            v,
            w,
        ) in edges:
            if not online[u] or not online[v]:
                continue
            g[u].append((v, w))
            l = min(l, w)
            r = max(r, w)

        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l if check(l) else -1
```

#### Java

```java
class Solution {
    int n;
    List<int[]>[] g;
    long k;

    boolean check(int mid) {
        long[] dist = new long[n];
        Arrays.fill(dist, Long.MAX_VALUE / 4);
        dist[0] = 0;

        PriorityQueue<long[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[0]));
        pq.offer(new long[] {0, 0});

        while (!pq.isEmpty()) {
            long[] cur = pq.poll();
            long d = cur[0];
            int u = (int) cur[1];

            if (d > k) return false;
            if (u == n - 1) return true;
            if (dist[u] < d) continue;

            for (int[] e : g[u]) {
                int v = e[0], w = e[1];
                if (w < mid) continue;

                long nd = d + w;
                if (nd < dist[v]) {
                    dist[v] = nd;
                    pq.offer(new long[] {nd, v});
                }
            }
        }

        return false;
    }

    public int findMaxPathScore(int[][] edges, boolean[] online, long k) {
        this.k = k;
        n = online.length;
        g = new ArrayList[n];
        for (int i = 0; i < n; i++) g[i] = new ArrayList<>();

        int l = Integer.MAX_VALUE;
        int r = 0;

        for (int[] e : edges) {
            int u = e[0], v = e[1], w = e[2];
            if (!online[u] || !online[v]) continue;

            g[u].add(new int[] {v, w});
            l = Math.min(l, w);
            r = Math.max(r, w);
        }

        while (l < r) {
            int mid = (l + r + 1) >>> 1;
            if (check(mid))
                l = mid;
            else
                r = mid - 1;
        }

        return check(l) ? l : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMaxPathScore(vector<vector<int>>& edges, vector<bool>& online, long long k) {
        int n = online.size();
        vector<vector<pair<int, int>>> g(n);

        int l = INT_MAX, r = 0;

        for (auto& e : edges) {
            int u = e[0], v = e[1], w = e[2];
            if (!online[u] || !online[v]) continue;
            g[u].push_back({v, w});
            l = min(l, w);
            r = max(r, w);
        }

        auto check = [&](int mid) -> bool {
            vector<long long> dist(n, LLONG_MAX / 4);
            dist[0] = 0;

            using P = pair<long long, int>;
            priority_queue<P, vector<P>, greater<P>> pq;
            pq.push({0, 0});

            while (!pq.empty()) {
                auto [d, u] = pq.top();
                pq.pop();

                if (d > k) return false;
                if (u == n - 1) return true;
                if (dist[u] < d) continue;

                for (auto& ed : g[u]) {
                    int v = ed.first, w = ed.second;
                    if (w < mid) continue;

                    long long nd = d + w;
                    if (nd < dist[v]) {
                        dist[v] = nd;
                        pq.push({nd, v});
                    }
                }
            }
            return false;
        };

        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid))
                l = mid;
            else
                r = mid - 1;
        }

        return check(l) ? l : -1;
    }
};
```

#### Go

```go
type Item struct {
    d int
    u int
}

type H []Item

func (h H) Len() int            { return len(h) }
func (h H) Less(i, j int) bool  { return h[i].d < h[j].d }
func (h H) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *H) Push(x any)         { *h = append(*h, x.(Item)) }
func (h *H) Pop() any           { x := (*h)[len(*h)-1]; *h = (*h)[:len(*h)-1]; return x }

func findMaxPathScore(edges [][]int, online []bool, k int64) int {
    n := len(online)
    g := make([][][]int, n)

    l, r := int(^uint(0)>>1), 0

    for _, e := range edges {
        u, v, w := e[0], e[1], e[2]
        if !online[u] || !online[v] {
            continue
        }
        g[u] = append(g[u], []int{v, w})
        if w < l {
            l = w
        }
        if w > r {
            r = w
        }
    }

    check := func(mid int) bool {
        const INF = int(^uint(0) >> 1)

        dist := make([]int, n)
        for i := range dist {
            dist[i] = INF
        }
        dist[0] = 0

        h := &H{}
        heap.Push(h, Item{0, 0})

        for h.Len() > 0 {
            cur := heap.Pop(h).(Item)
            d, u := cur.d, cur.u

            if int64(d) > k {
                return false
            }
            if u == n-1 {
                return true
            }
            if dist[u] < d {
                continue
            }

            for _, e := range g[u] {
                v, w := e[0], e[1]
                if w < mid {
                    continue
                }
                nd := d + w
                if nd < dist[v] {
                    dist[v] = nd
                    heap.Push(h, Item{nd, v})
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

    if check(l) {
        return l
    }
    return -1
}
```

#### TypeScript

```ts
function findMaxPathScore(edges: number[][], online: boolean[], k: number): number {
    const n = online.length;
    const g: [number, number][][] = Array.from({ length: n }, () => []);

    let l = Number.MAX_SAFE_INTEGER;
    let r = 0;

    for (const [u, v, w] of edges) {
        if (!online[u] || !online[v]) continue;
        g[u].push([v, w]);
        l = Math.min(l, w);
        r = Math.max(r, w);
    }

    const check = (mid: number): boolean => {
        const INF = Number.MAX_SAFE_INTEGER / 2;
        const dist = new Array<number>(n).fill(INF);
        dist[0] = 0;

        const pq = new PriorityQueue<[number, number]>((a, b) => a[0] - b[0]);
        pq.enqueue([0, 0]);

        while (!pq.isEmpty()) {
            const [d, u] = pq.dequeue();

            if (d > k) return false;
            if (u === n - 1) return true;
            if (dist[u] < d) continue;

            for (const [v, w] of g[u]) {
                if (w < mid) continue;

                const nd = d + w;
                if (nd < dist[v]) {
                    dist[v] = nd;
                    pq.enqueue([nd, v]);
                }
            }
        }

        return false;
    };

    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }

    return check(l) ? l : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
