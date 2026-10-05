---
comments: true
difficulty: Hard
tags:
    - Graph
    - Array
    - Math
    - Min-Cost Flow
---

<!-- problem:start -->

# [4004. Minimum Moves to Balance Circular Array II 🔒](https://leetcode.com/problems/minimum-moves-to-balance-circular-array-ii)

[中文文档](/solution/4000-4099/4004.Minimum%20Moves%20to%20Balance%20Circular%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <span data-keyword="circular-array">mảng vòng</span> <code>balance</code> có độ dài <code>n</code>, trong đó <code>balance[i]</code> là số dư ròng của người thứ <code>i</code>.</p>

<p>Trong một thao tác, một người có thể chuyển <strong>đúng</strong> 1 đơn vị số dư cho người hàng xóm bên trái hoặc bên phải.</p>

<p>Hãy trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để số dư của mọi người đều <strong>không âm</strong>. Nếu không thể, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">balance = [-1,2,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi thao tác tối ưu là:</p>

<ul>
	<li>Chuyển 1 đơn vị từ <code>i = 1</code> tới <code>i = 0</code>, thu được <code>balance = [0, 1, -1]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 1</code> tới <code>i = 2</code>, thu được <code>balance = [0, 0, 0]</code></li>
</ul>

<p>Vì vậy, số thao tác nhỏ nhất cần thực hiện là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">balance = [4,-1,-2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi thao tác tối ưu là:</p>

<ul>
	<li>Chuyển 1 đơn vị từ <code>i = 0</code> tới <code>i = 1</code>, thu được <code>balance = [3, 0, -2]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 0</code> tới <code>i = 2</code>, thu được <code>balance = [2, 0, -1]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 0</code> tới <code>i = 2</code>, thu được <code>balance = [1, 0, 0]</code></li>
</ul>

<p>Vì vậy, số thao tác nhỏ nhất cần thực hiện là 3.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">balance = [-3,-3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể làm cho mọi số dư đều không âm với <code>balance = [-3, -3, 5]</code>, nên đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == balance.length &lt;= 1000</code></li>
	<li><code>-10<sup>5</sup> &lt;= balance[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Luồng cực đại chi phí nhỏ nhất

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác chuyển một đơn vị tới hàng xóm trên vòng tròn, với $n\le 1000$. Việc tìm kiếm theo chuỗi thao tác không thể mở rộng.
>
> Các ô dư phải gửi đơn vị tới các ô thiếu, và số bước một đơn vị di chuyển chính là số thao tác. Đây là bài toán min-cost flow với các ô dư làm nguồn, các ô thiếu làm đích, các cạnh trên vòng có sức chứa vô hạn và chi phí $1$; lượng flow cần gửi bằng tổng phần thiếu. Nếu tổng số dư âm thì bài toán vô nghiệm.
>
> Vòng tròn liên thông hai chiều với chi phí đơn vị, nên có thể dùng các lần tăng flow liên tiếp bằng SPFA. Với $n$ trong giới hạn này, cận xấu nhất $O(n^3)$ là chấp nhận được.

<!-- thinking:end -->

Gọi $n$ là độ dài của mảng $\textit{balance}$. Nếu tổng tất cả số dư âm, không thể làm số dư của mọi người không âm, nên trả về $-1$ ngay.

Ngược lại, ta mô hình hóa bài toán thành bài toán **min-cost flow**:

- Tạo source $s$ và sink $t$;
- Với mỗi người $i$ có $\textit{balance}[i] > 0$ (người dư), thêm cạnh từ $s$ tới $i$ có sức chứa $\textit{balance}[i]$ và chi phí đơn vị $0$;
- Với mỗi người $i$ có $\textit{balance}[i] < 0$ (người thiếu), thêm cạnh từ $i$ tới $t$ có sức chứa $-\textit{balance}[i]$ và chi phí đơn vị $0$;
- Với mỗi $i$, thêm cạnh từ $i$ tới mỗi trong hai hàng xóm, có sức chứa vô hạn và chi phí đơn vị $1$, biểu diễn việc chuyển $1$ đơn vị số dư tới hàng xóm cần $1$ thao tác.

Gọi $\textit{totalDeficit} = \sum_{\textit{balance}[i] < 0} (-\textit{balance}[i])$ là tổng phần thiếu. Đáp án là chi phí nhỏ nhất khi gửi $\textit{totalDeficit}$ đơn vị flow từ $s$ tới $t$. Vì các cạnh của vòng tròn kết nối mọi người theo cả hai hướng, toàn bộ flow cần thiết luôn có thể được chuyển đi nếu tổng số dư không âm. Ta dùng thuật toán tăng đường đi ngắn nhất liên tiếp dựa trên SPFA để giải bài toán min-cost flow.

Lưu ý rằng mỗi lần tăng flow sẽ đẩy toàn bộ flow tại nút thắt trên một đường đi ngắn nhất, thay vì chỉ đẩy $1$ đơn vị: cạnh nút thắt hoặc là cạnh nối với source hoặc sink (sau đó bị bão hòa), hoặc là một cạnh vòng tròn ngược (định tuyến lại toàn bộ flow đang có trên cạnh xuôi tương ứng). Vì vậy, số lần tăng flow không phụ thuộc vào độ lớn của các số dư và có bậc $O(n)$ trong giới hạn bài này. Mỗi lần tăng flow dùng SPFA để tìm đường tăng ngắn nhất, mất $O(VE)$ trong trường hợp xấu nhất, với $V = n + 2$ và $E = O(n)$.

Độ phức tạp thời gian trong trường hợp xấu nhất là $O(n^3)$ và độ phức tạp không gian là $O(n)$. Cần lưu ý $O(n^3)$ là một cận trên rất bảo thủ: một mặt, thực tế chỉ có $O(n)$ lần tăng flow; mặt khác, đồ thị trong bài là một vòng có chi phí đơn vị, trên đó SPFA hoạt động gần như BFS — trung bình mỗi nút chỉ được lấy ra khỏi hàng đợi một số lần hằng số, nên một lần tăng flow thực tế tốn khoảng $O(n)$. Tổng số phép tính thực tế do đó khoảng $O(n^2)$, tương đương khoảng $10^7$ phép tính đơn giản khi $n = 1000$, đủ nhanh để vượt qua bài này. Nếu cần một cận được chứng minh chặt chẽ, có thể thay SPFA bằng Dijkstra cùng với thế Johnson, cho độ phức tạp $O(n^2 \log n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, balance: List[int]) -> int:
        total_balance = sum(balance)
        if total_balance < 0:
            return -1

        n = len(balance)
        total_deficit = sum(-x for x in balance if x < 0)
        if total_deficit == 0:
            return 0

        source = n
        sink = n + 1
        num_nodes = n + 2

        graph = [[] for _ in range(num_nodes)]

        def add_edge(u, v, cap, cost):
            graph[u].append([v, cap, cost, len(graph[v])])
            graph[v].append([u, 0, -cost, len(graph[u]) - 1])

        for i in range(n):
            if balance[i] > 0:
                add_edge(source, i, balance[i], 0)
            elif balance[i] < 0:
                add_edge(i, sink, -balance[i], 0)

            add_edge(i, (i + 1) % n, inf, 1)
            add_edge(i, (i - 1 + n) % n, inf, 1)

        total_cost = 0
        current_flow = 0

        while current_flow < total_deficit:
            dist = [inf] * num_nodes
            parent_node = [-1] * num_nodes
            parent_edge = [-1] * num_nodes
            in_queue = [False] * num_nodes

            queue = deque([source])
            dist[source] = 0
            in_queue[source] = True

            while queue:
                u = queue.popleft()
                in_queue[u] = False

                for idx, (v, cap, cost, _) in enumerate(graph[u]):
                    if cap > 0 and dist[v] > dist[u] + cost:
                        dist[v] = dist[u] + cost
                        parent_node[v] = u
                        parent_edge[v] = idx
                        if not in_queue[v]:
                            queue.append(v)
                            in_queue[v] = True

            if dist[sink] == inf:
                break

            push_flow = total_deficit - current_flow
            curr = sink
            while curr != source:
                p = parent_node[curr]
                idx = parent_edge[curr]
                push_flow = min(push_flow, graph[p][idx][1])
                curr = p

            curr = sink
            while curr != source:
                p = parent_node[curr]
                idx = parent_edge[curr]
                rev_idx = graph[p][idx][3]
                graph[p][idx][1] -= push_flow
                graph[curr][rev_idx][1] += push_flow
                curr = p

            current_flow += push_flow
            total_cost += push_flow * dist[sink]

        return total_cost if current_flow == total_deficit else -1
```

#### Java

```java
class MinCostMaxFlow {

    static class Edge {
        int to;
        int cap;
        int cost;
        int rev;

        Edge(int to, int cap, int cost, int rev) {
            this.to = to;
            this.cap = cap;
            this.cost = cost;
            this.rev = rev;
        }
    }

    private static final int INF = 1 << 29;

    private final int n;
    private final List<Edge>[] graph;

    public MinCostMaxFlow(int n) {
        this.n = n;
        graph = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
        }
    }

    public void addEdge(int u, int v, int cap, int cost) {
        graph[u].add(new Edge(v, cap, cost, graph[v].size()));
        graph[v].add(new Edge(u, 0, -cost, graph[u].size() - 1));
    }

    public long minCostFlow(int source, int sink, int maxFlow) {
        long totalCost = 0;
        int currentFlow = 0;

        while (currentFlow < maxFlow) {
            int[] dist = new int[n];
            Arrays.fill(dist, INF);

            int[] parentNode = new int[n];
            int[] parentEdge = new int[n];
            boolean[] inQueue = new boolean[n];

            Arrays.fill(parentNode, -1);
            Arrays.fill(parentEdge, -1);

            Queue<Integer> queue = new ArrayDeque<>();
            queue.offer(source);
            dist[source] = 0;
            inQueue[source] = true;

            while (!queue.isEmpty()) {
                int u = queue.poll();
                inQueue[u] = false;

                for (int i = 0; i < graph[u].size(); i++) {
                    Edge e = graph[u].get(i);
                    if (e.cap > 0 && dist[e.to] > dist[u] + e.cost) {
                        dist[e.to] = dist[u] + e.cost;
                        parentNode[e.to] = u;
                        parentEdge[e.to] = i;

                        if (!inQueue[e.to]) {
                            inQueue[e.to] = true;
                            queue.offer(e.to);
                        }
                    }
                }
            }

            if (dist[sink] == INF) {
                return -1;
            }

            int pushFlow = maxFlow - currentFlow;

            for (int cur = sink; cur != source; cur = parentNode[cur]) {
                Edge e = graph[parentNode[cur]].get(parentEdge[cur]);
                pushFlow = Math.min(pushFlow, e.cap);
            }

            for (int cur = sink; cur != source; cur = parentNode[cur]) {
                Edge e = graph[parentNode[cur]].get(parentEdge[cur]);
                e.cap -= pushFlow;
                graph[cur].get(e.rev).cap += pushFlow;
            }

            currentFlow += pushFlow;
            totalCost += 1L * pushFlow * dist[sink];
        }

        return totalCost;
    }
}

class Solution {

    public long minMoves(int[] balance) {
        int totalBalance = 0;
        int totalDeficit = 0;

        for (int x : balance) {
            totalBalance += x;
            if (x < 0) {
                totalDeficit += -x;
            }
        }

        if (totalBalance < 0) {
            return -1;
        }

        if (totalDeficit == 0) {
            return 0;
        }

        int n = balance.length;
        int source = n;
        int sink = n + 1;
        int INF = 1 << 29;

        MinCostMaxFlow mcmf = new MinCostMaxFlow(n + 2);

        for (int i = 0; i < n; i++) {
            if (balance[i] > 0) {
                mcmf.addEdge(source, i, balance[i], 0);
            } else if (balance[i] < 0) {
                mcmf.addEdge(i, sink, -balance[i], 0);
            }

            mcmf.addEdge(i, (i + 1) % n, INF, 1);
            mcmf.addEdge(i, (i - 1 + n) % n, INF, 1);
        }

        return mcmf.minCostFlow(source, sink, totalDeficit);
    }
}
```

#### C++

```cpp
class MinCostMaxFlow {
public:
    struct Edge {
        int to, cap, cost, rev;

        Edge(int to, int cap, int cost, int rev)
            : to(to)
            , cap(cap)
            , cost(cost)
            , rev(rev) {}
    };

    static constexpr int INF = 1e9;

    int n;
    vector<vector<Edge>> graph;

    MinCostMaxFlow(int n)
        : n(n)
        , graph(n) {}

    void addEdge(int u, int v, int cap, int cost) {
        graph[u].emplace_back(v, cap, cost, graph[v].size());
        graph[v].emplace_back(u, 0, -cost, graph[u].size() - 1);
    }

    long long minCostFlow(int source, int sink, int maxFlow) {
        long long totalCost = 0;
        int currentFlow = 0;

        while (currentFlow < maxFlow) {
            vector<int> dist(n, INF);
            vector<int> parentNode(n, -1);
            vector<int> parentEdge(n, -1);
            vector<bool> inQueue(n, false);

            queue<int> q;
            q.push(source);
            dist[source] = 0;
            inQueue[source] = true;

            while (!q.empty()) {
                int u = q.front();
                q.pop();
                inQueue[u] = false;

                for (int i = 0; i < graph[u].size(); i++) {
                    Edge& e = graph[u][i];
                    if (e.cap > 0 && dist[e.to] > dist[u] + e.cost) {
                        dist[e.to] = dist[u] + e.cost;
                        parentNode[e.to] = u;
                        parentEdge[e.to] = i;

                        if (!inQueue[e.to]) {
                            inQueue[e.to] = true;
                            q.push(e.to);
                        }
                    }
                }
            }

            if (dist[sink] == INF) {
                return -1;
            }

            int pushFlow = maxFlow - currentFlow;

            for (int cur = sink; cur != source; cur = parentNode[cur]) {
                Edge& e = graph[parentNode[cur]][parentEdge[cur]];
                pushFlow = min(pushFlow, e.cap);
            }

            for (int cur = sink; cur != source; cur = parentNode[cur]) {
                Edge& e = graph[parentNode[cur]][parentEdge[cur]];
                e.cap -= pushFlow;
                graph[cur][e.rev].cap += pushFlow;
            }

            currentFlow += pushFlow;
            totalCost += 1LL * pushFlow * dist[sink];
        }

        return totalCost;
    }
};

class Solution {
public:
    long long minMoves(vector<int>& balance) {
        int totalBalance = accumulate(balance.begin(), balance.end(), 0);
        if (totalBalance < 0) {
            return -1;
        }

        int totalDeficit = 0;
        for (int x : balance) {
            if (x < 0) {
                totalDeficit += -x;
            }
        }

        if (totalDeficit == 0) {
            return 0;
        }

        int n = balance.size();
        int source = n;
        int sink = n + 1;

        MinCostMaxFlow mcmf(n + 2);

        for (int i = 0; i < n; i++) {
            if (balance[i] > 0) {
                mcmf.addEdge(source, i, balance[i], 0);
            } else if (balance[i] < 0) {
                mcmf.addEdge(i, sink, -balance[i], 0);
            }

            mcmf.addEdge(i, (i + 1) % n, MinCostMaxFlow::INF, 1);
            mcmf.addEdge(i, (i - 1 + n) % n, MinCostMaxFlow::INF, 1);
        }

        return mcmf.minCostFlow(source, sink, totalDeficit);
    }
};
```

#### Go

```go
type Edge struct {
	to   int
	cap  int
	cost int
	rev  int
}

type MinCostMaxFlow struct {
	n     int
	graph [][]Edge
}

const INF = int(1e9)

func NewMinCostMaxFlow(n int) *MinCostMaxFlow {
	return &MinCostMaxFlow{
		n:     n,
		graph: make([][]Edge, n),
	}
}

func (m *MinCostMaxFlow) AddEdge(u, v, cap, cost int) {
	m.graph[u] = append(m.graph[u], Edge{
		to:   v,
		cap:  cap,
		cost: cost,
		rev:  len(m.graph[v]),
	})
	m.graph[v] = append(m.graph[v], Edge{
		to:   u,
		cap:  0,
		cost: -cost,
		rev:  len(m.graph[u]) - 1,
	})
}

func (m *MinCostMaxFlow) MinCostFlow(source, sink, maxFlow int) int64 {
	var totalCost int64
	currentFlow := 0

	for currentFlow < maxFlow {
		dist := make([]int, m.n)
		parentNode := make([]int, m.n)
		parentEdge := make([]int, m.n)
		inQueue := make([]bool, m.n)

		for i := 0; i < m.n; i++ {
			dist[i] = INF
			parentNode[i] = -1
			parentEdge[i] = -1
		}

		queue := []int{source}
		head := 0
		dist[source] = 0
		inQueue[source] = true

		for head < len(queue) {
			u := queue[head]
			head++
			inQueue[u] = false

			for i, e := range m.graph[u] {
				if e.cap > 0 && dist[e.to] > dist[u]+e.cost {
					dist[e.to] = dist[u] + e.cost
					parentNode[e.to] = u
					parentEdge[e.to] = i

					if !inQueue[e.to] {
						inQueue[e.to] = true
						queue = append(queue, e.to)
					}
				}
			}
		}

		if dist[sink] == INF {
			return -1
		}

		pushFlow := maxFlow - currentFlow

		for cur := sink; cur != source; cur = parentNode[cur] {
			e := &m.graph[parentNode[cur]][parentEdge[cur]]
			if e.cap < pushFlow {
				pushFlow = e.cap
			}
		}

		for cur := sink; cur != source; cur = parentNode[cur] {
			p := parentNode[cur]
			idx := parentEdge[cur]
			rev := m.graph[p][idx].rev

			m.graph[p][idx].cap -= pushFlow
			m.graph[cur][rev].cap += pushFlow
		}

		currentFlow += pushFlow
		totalCost += int64(pushFlow * dist[sink])
	}

	return totalCost
}

func minMoves(balance []int) int64 {
	totalBalance := 0
	totalDeficit := 0

	for _, x := range balance {
		totalBalance += x
		if x < 0 {
			totalDeficit += -x
		}
	}

	if totalBalance < 0 {
		return -1
	}

	if totalDeficit == 0 {
		return 0
	}

	n := len(balance)
	source := n
	sink := n + 1

	mcmf := NewMinCostMaxFlow(n + 2)

	for i, x := range balance {
		if x > 0 {
			mcmf.AddEdge(source, i, x, 0)
		} else if x < 0 {
			mcmf.AddEdge(i, sink, -x, 0)
		}

		mcmf.AddEdge(i, (i+1)%n, INF, 1)
		mcmf.AddEdge(i, (i-1+n)%n, INF, 1)
	}

	return mcmf.MinCostFlow(source, sink, totalDeficit)
}
```

#### TypeScript

```ts
class Edge {
    to: number;
    cap: number;
    cost: number;
    rev: number;

    constructor(to: number, cap: number, cost: number, rev: number) {
        this.to = to;
        this.cap = cap;
        this.cost = cost;
        this.rev = rev;
    }
}

class MinCostMaxFlow {
    private n: number;
    private graph: Edge[][];

    static readonly INF = 1e9;

    constructor(n: number) {
        this.n = n;
        this.graph = Array.from({ length: n }, () => []);
    }

    addEdge(u: number, v: number, cap: number, cost: number): void {
        this.graph[u].push(new Edge(v, cap, cost, this.graph[v].length));

        this.graph[v].push(new Edge(u, 0, -cost, this.graph[u].length - 1));
    }

    minCostFlow(source: number, sink: number, maxFlow: number): number {
        let totalCost = 0;
        let currentFlow = 0;

        while (currentFlow < maxFlow) {
            const dist = new Array<number>(this.n).fill(MinCostMaxFlow.INF);

            const parentNode = new Array<number>(this.n).fill(-1);

            const parentEdge = new Array<number>(this.n).fill(-1);

            const inQueue = new Array<boolean>(this.n).fill(false);

            const queue: number[] = [];

            queue.push(source);
            dist[source] = 0;
            inQueue[source] = true;

            let head = 0;

            while (head < queue.length) {
                const u = queue[head++];
                inQueue[u] = false;

                for (let i = 0; i < this.graph[u].length; i++) {
                    const e = this.graph[u][i];

                    if (e.cap > 0 && dist[e.to] > dist[u] + e.cost) {
                        dist[e.to] = dist[u] + e.cost;
                        parentNode[e.to] = u;
                        parentEdge[e.to] = i;

                        if (!inQueue[e.to]) {
                            inQueue[e.to] = true;
                            queue.push(e.to);
                        }
                    }
                }
            }

            if (dist[sink] === MinCostMaxFlow.INF) {
                return -1;
            }

            let pushFlow = maxFlow - currentFlow;

            for (let cur = sink; cur !== source; cur = parentNode[cur]) {
                const e = this.graph[parentNode[cur]][parentEdge[cur]];
                pushFlow = Math.min(pushFlow, e.cap);
            }

            for (let cur = sink; cur !== source; cur = parentNode[cur]) {
                const p = parentNode[cur];
                const idx = parentEdge[cur];

                const e = this.graph[p][idx];

                e.cap -= pushFlow;
                this.graph[cur][e.rev].cap += pushFlow;
            }

            currentFlow += pushFlow;
            totalCost += pushFlow * dist[sink];
        }

        return totalCost;
    }
}

function minMoves(balance: number[]): number {
    let totalBalance = 0;
    let totalDeficit = 0;

    for (const x of balance) {
        totalBalance += x;
        if (x < 0) {
            totalDeficit += -x;
        }
    }

    if (totalBalance < 0) {
        return -1;
    }

    if (totalDeficit === 0) {
        return 0;
    }

    const n = balance.length;

    const source = n;
    const sink = n + 1;

    const mcmf = new MinCostMaxFlow(n + 2);

    for (let i = 0; i < n; i++) {
        if (balance[i] > 0) {
            mcmf.addEdge(source, i, balance[i], 0);
        } else if (balance[i] < 0) {
            mcmf.addEdge(i, sink, -balance[i], 0);
        }

        mcmf.addEdge(i, (i + 1) % n, MinCostMaxFlow.INF, 1);

        mcmf.addEdge(i, (i - 1 + n) % n, MinCostMaxFlow.INF, 1);
    }

    return mcmf.minCostFlow(source, sink, totalDeficit);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
