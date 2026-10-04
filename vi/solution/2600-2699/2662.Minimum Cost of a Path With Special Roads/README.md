---
comments: true
difficulty: Medium
rating: 2153
source: Weekly Contest 343 Q3
tags:
    - Graph
    - Array
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2662. Minimum Cost of a Path With Special Roads](https://leetcode.com/problems/minimum-cost-of-a-path-with-special-roads)

[中文文档](/solution/2600-2699/2662.Minimum%20Cost%20of%20a%20Path%20With%20Special%20Roads/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>start</code>, trong đó <code>start = [startX, startY]</code> biểu diễn vị trí ban đầu <code>(startX, startY)</code> của bạn trong không gian 2D. Bạn cũng được cho mảng <code>target</code>, trong đó <code>target = [targetX, targetY]</code> biểu diễn vị trí đích <code>(targetX, targetY)</code>.</p>

<p><strong>Chi phí</strong> di chuyển từ vị trí <code>(x1, y1)</code> đến một vị trí bất kỳ khác <code>(x2, y2)</code> trong không gian là <code>|x2 - x1| + |y2 - y1|</code>.</p>

<p>Ngoài ra còn có một số <strong>đường đi đặc biệt</strong>. Bạn được cho mảng 2D <code>specialRoads</code>, trong đó <code>specialRoads[i] = [x1<sub>i</sub>, y1<sub>i</sub>, x2<sub>i</sub>, y2<sub>i</sub>, cost<sub>i</sub>]</code> cho biết đường đi đặc biệt thứ <code>i<sup>th</sup></code> là <strong>một chiều</strong>, đi từ <code>(x1<sub>i</sub>, y1<sub>i</sub>)</code> đến <code>(x2<sub>i</sub>, y2<sub>i</sub>)</code> với chi phí bằng <code>cost<sub>i</sub></code>. Bạn có thể sử dụng mỗi đường đi đặc biệt bao nhiêu lần cũng được.</p>

<p>Trả về chi phí <strong>nhỏ nhất</strong> cần thiết để đi từ <code>(startX, startY)</code> đến <code>(targetX, targetY)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">start = [1,1], target = [4,5], specialRoads = [[1,2,3,3,2],[3,4,4,5,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>Từ (1,1) đến (1,2) với chi phí |1 - 1| + |2 - 1| = 1.</li>
	<li>Từ (1,2) đến (3,3). Sử dụng <code><span class="example-io">specialRoads[0]</span></code><span class="example-io"> với</span><span class="example-io"> chi phí 2.</span></li>
	<li><span class="example-io">Từ (3,3) đến (3,4) với chi phí |3 - 3| + |4 - 3| = 1.</span></li>
	<li><span class="example-io">Từ (3,4) đến (4,5). Sử dụng </span><code><span class="example-io">specialRoads[1]</span></code><span class="example-io"> với chi phí</span><span class="example-io"> 1.</span></li>
</ol>

<p><span class="example-io">Vậy tổng chi phí là 1 + 2 + 1 + 1 = 5.</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">start = [3,2], target = [5,7], specialRoads = [[5,7,3,2,1],[3,2,3,4,4],[3,3,5,5,5],[3,4,5,6,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tối ưu là không sử dụng cạnh đặc biệt nào và đi trực tiếp từ vị trí bắt đầu đến vị trí kết thúc với chi phí |5 - 3| + |7 - 2| = 7.</p>

<p>Lưu ý rằng <span class="example-io"><code>specialRoads[0]</code> là đường một chiều từ (5,7) đến (3,2).</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">start = [1,1], target = [10,4], specialRoads = [[4,2,1,1,3],[1,2,7,4,4],[10,3,6,1,2],[6,1,1,2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>Từ (1,1) đến (1,2) với chi phí |1 - 1| + |2 - 1| = 1.</li>
	<li>Từ (1,2) đến (7,4). Sử dụng <code><span class="example-io">specialRoads[1]</span></code><span class="example-io"> với chi phí</span><span class="example-io"> 4.</span></li>
	<li>Từ (7,4) đến (10,4) với chi phí |10 - 7| + |4 - 4| = 3.</li>
</ol>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>start.length == target.length == 2</code></li>
	<li><code>1 &lt;= startX &lt;= targetX &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= startY &lt;= targetY &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= specialRoads.length &lt;= 200</code></li>
	<li><code>specialRoads[i].length == 5</code></li>
	<li><code>startX &lt;= x1<sub>i</sub>, x2<sub>i</sub> &lt;= targetX</code></li>
	<li><code>startY &lt;= y1<sub>i</sub>, y2<sub>i</sub> &lt;= targetY</code></li>
	<li><code>1 &lt;= cost<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dijkstra

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể tự do di chuyển theo khoảng cách Manhattan hoặc sử dụng các đường đi đặc biệt đã cho. Nếu xem mọi điểm trên lưới là một trạng thái thì số trạng thái là vô hạn. Một lời giải tối ưu chỉ cần đổi hướng tại điểm bắt đầu, điểm đích và các đầu mút của đường đi đặc biệt.
>
> Chạy Dijkstra từ điểm bắt đầu: tại $(x,y)$, ta có thể trả khoảng cách Manhattan đến đích, hoặc đi đến lối vào của một đường đi đặc biệt rồi chuyển đến lối ra. Các điểm đã thăm sẽ không được mở rộng lại.
>
> Có nhiều nhất $200$ đường đi đặc biệt, nên heap vẫn nằm trong giới hạn có thể xử lý.

<!-- thinking:end -->

Ta nhận thấy rằng với mỗi tọa độ $(x, y)$ đã đi qua, giả sử chi phí nhỏ nhất từ điểm bắt đầu đến $(x, y)$ là $d$. Nếu chọn di chuyển trực tiếp đến $(targetX, targetY)$, tổng chi phí sẽ là $d + |x - targetX| + |y - targetY|$. Nếu chọn đi qua đường đặc biệt $(x_1, y_1) \rightarrow (x_2, y_2)$, ta cần trả $|x - x_1| + |y - y_1| + cost$ để đi từ $(x, y)$ đến $(x_2, y_2)$.

Do đó, ta có thể sử dụng thuật toán Dijkstra để tìm chi phí nhỏ nhất từ điểm bắt đầu đến mọi điểm, sau đó chọn giá trị nhỏ nhất trong số đó.

Ta định nghĩa một priority queue $q$, mỗi phần tử trong queue là một bộ ba $(d, x, y)$, biểu diễn chi phí nhỏ nhất từ điểm bắt đầu đến $(x, y)$ là $d$. Ban đầu, ta thêm $(0, startX, startY)$ vào queue.

Ở mỗi bước, ta lấy phần tử đầu tiên $(d, x, y)$ ra khỏi queue. Khi đó, ta có thể cập nhật đáp án, tức là $ans = \min(ans, d + dist(x, y, targetX, targetY))$. Sau đó, ta duyệt qua tất cả đường đặc biệt $(x_1, y_1) \rightarrow (x_2, y_2)$ và thêm $(d + dist(x, y, x_1, y_1) + cost, x_2, y_2)$ vào queue.

Cuối cùng, khi queue rỗng, ta thu được đáp án.

Độ phức tạp thời gian là $O(n^2 \times \log n)$, độ phức tạp không gian là $O(n^2)$. Trong đó $n$ là số đường đặc biệt.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(
        self, start: List[int], target: List[int], specialRoads: List[List[int]]
    ) -> int:
        def dist(x1: int, y1: int, x2: int, y2: int) -> int:
            return abs(x1 - x2) + abs(y1 - y2)

        q = [(0, start[0], start[1])]
        vis = set()
        ans = inf
        while q:
            d, x, y = heappop(q)
            if (x, y) in vis:
                continue
            vis.add((x, y))
            ans = min(ans, d + dist(x, y, *target))
            for x1, y1, x2, y2, cost in specialRoads:
                heappush(q, (d + dist(x, y, x1, y1) + cost, x2, y2))
        return ans
```

#### Java

```java
class Solution {
    public int minimumCost(int[] start, int[] target, int[][] specialRoads) {
        int ans = 1 << 30;
        int n = 1000000;
        PriorityQueue<int[]> q = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        Set<Long> vis = new HashSet<>();
        q.offer(new int[] {0, start[0], start[1]});
        while (!q.isEmpty()) {
            var p = q.poll();
            int x = p[1], y = p[2];
            long k = 1L * x * n + y;
            if (vis.contains(k)) {
                continue;
            }
            vis.add(k);
            int d = p[0];
            ans = Math.min(ans, d + dist(x, y, target[0], target[1]));
            for (var r : specialRoads) {
                int x1 = r[0], y1 = r[1], x2 = r[2], y2 = r[3], cost = r[4];
                q.offer(new int[] {d + dist(x, y, x1, y1) + cost, x2, y2});
            }
        }
        return ans;
    }

    private int dist(int x1, int y1, int x2, int y2) {
        return Math.abs(x1 - x2) + Math.abs(y1 - y2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCost(vector<int>& start, vector<int>& target, vector<vector<int>>& specialRoads) {
        auto dist = [](int x1, int y1, int x2, int y2) {
            return abs(x1 - x2) + abs(y1 - y2);
        };
        int ans = 1 << 30;
        int n = 1e6;
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> pq;
        pq.push({0, start[0], start[1]});
        unordered_set<long long> vis;
        while (!pq.empty()) {
            auto [d, x, y] = pq.top();
            pq.pop();
            long long k = 1LL * x * n + y;
            if (vis.count(k)) {
                continue;
            }
            vis.insert(k);
            ans = min(ans, d + dist(x, y, target[0], target[1]));
            for (auto& r : specialRoads) {
                int x1 = r[0], y1 = r[1], x2 = r[2], y2 = r[3], cost = r[4];
                pq.push({d + dist(x, y, x1, y1) + cost, x2, y2});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumCost(start []int, target []int, specialRoads [][]int) int {
	ans := 1 << 30
	const n int = 1e6
	pq := hp{{0, start[0], start[1]}}
	vis := map[int]bool{}
	for len(pq) > 0 {
		p := pq[0]
		heap.Pop(&pq)
		d, x, y := p.d, p.x, p.y
		if vis[x*n+y] {
			continue
		}
		vis[x*n+y] = true
		ans = min(ans, d+dist(x, y, target[0], target[1]))
		for _, r := range specialRoads {
			x1, y1, x2, y2, cost := r[0], r[1], r[2], r[3], r[4]
			heap.Push(&pq, tuple{d + dist(x, y, x1, y1) + cost, x2, y2})
		}
	}
	return ans
}

func dist(x1, y1, x2, y2 int) int {
	return abs(x1-x2) + abs(y1-y2)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

type tuple struct {
	d, x, y int
}
type hp []tuple

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].d < h[j].d }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(tuple)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function minimumCost(start: number[], target: number[], specialRoads: number[][]): number {
    const dist = (x1: number, y1: number, x2: number, y2: number): number => {
        return Math.abs(x1 - x2) + Math.abs(y1 - y2);
    };
    const q = new Heap<[number, number, number]>((a, b) => a[0] - b[0]);
    q.push([0, start[0], start[1]]);
    const n = 1000000;
    const vis: Set<number> = new Set();
    let ans = 1 << 30;
    while (q.size()) {
        const [d, x, y] = q.pop();
        const k = x * n + y;
        if (vis.has(k)) {
            continue;
        }
        vis.add(k);
        ans = Math.min(ans, d + dist(x, y, target[0], target[1]));
        for (const [x1, y1, x2, y2, cost] of specialRoads) {
            q.push([d + dist(x, y, x1, y1) + cost, x2, y2]);
        }
    }
    return ans;
}

type Compare<T> = (lhs: T, rhs: T) => number;

class Heap<T = number> {
    data: Array<T | null>;
    lt: (i: number, j: number) => boolean;
    constructor();
    constructor(data: T[]);
    constructor(compare: Compare<T>);
    constructor(data: T[], compare: Compare<T>);
    constructor(data: T[] | Compare<T>, compare?: (lhs: T, rhs: T) => number);
    constructor(
        data: T[] | Compare<T> = [],
        compare: Compare<T> = (lhs: T, rhs: T) => (lhs < rhs ? -1 : lhs > rhs ? 1 : 0),
    ) {
        if (typeof data === 'function') {
            compare = data;
            data = [];
        }
        this.data = [null, ...data];
        this.lt = (i, j) => compare(this.data[i]!, this.data[j]!) < 0;
        for (let i = this.size(); i > 0; i--) this.heapify(i);
    }

    size(): number {
        return this.data.length - 1;
    }

    push(v: T): void {
        this.data.push(v);
        let i = this.size();
        while (i >> 1 !== 0 && this.lt(i, i >> 1)) this.swap(i, (i >>= 1));
    }

    pop(): T {
        this.swap(1, this.size());
        const top = this.data.pop();
        this.heapify(1);
        return top!;
    }

    top(): T {
        return this.data[1]!;
    }
    heapify(i: number): void {
        while (true) {
            let min = i;
            const [l, r, n] = [i * 2, i * 2 + 1, this.data.length];
            if (l < n && this.lt(l, min)) min = l;
            if (r < n && this.lt(r, min)) min = r;
            if (min !== i) {
                this.swap(i, min);
                i = min;
            } else break;
        }
    }

    clear(): void {
        this.data = [null];
    }

    private swap(i: number, j: number): void {
        const d = this.data;
        [d[i], d[j]] = [d[j], d[i]];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
