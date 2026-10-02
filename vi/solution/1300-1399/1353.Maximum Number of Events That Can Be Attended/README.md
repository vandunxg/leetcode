---
comments: true
difficulty: Medium
rating: 2015
source: Weekly Contest 176 Q3
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1353. Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended)

[中文文档](/solution/1300-1399/1353.Maximum%20Number%20of%20Events%20That%20Can%20Be%20Attended/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>events</code>, trong đó <code>events[i] = [startDay<sub>i</sub>, endDay<sub>i</sub>]</code>. Mỗi sự kiện <code>i</code> bắt đầu vào <code>startDay<sub>i</sub></code> và kết thúc vào <code>endDay<sub>i</sub></code>.</p>

<p>Bạn có thể tham dự sự kiện <code>i</code> vào ngày <code>d</code> bất kỳ thỏa mãn <code>startDay<sub>i</sub> &lt;= d &lt;= endDay<sub>i</sub></code>. Mỗi ngày <code>d</code>, bạn chỉ có thể tham dự tối đa một sự kiện.</p>

<p>Trả về <em>số lượng sự kiện lớn nhất mà bạn có thể tham dự</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1353.Maximum%20Number%20of%20Events%20That%20Can%20Be%20Attended/images/e1.png" style="width: 400px; height: 267px;" />
<pre>
<strong>Đầu vào:</strong> events = [[1,2],[2,3],[3,4]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Bạn có thể tham dự cả ba sự kiện.
Một cách tham dự tất cả các sự kiện như sau.
Tham dự sự kiện thứ nhất vào ngày 1.
Tham dự sự kiện thứ hai vào ngày 2.
Tham dự sự kiện thứ ba vào ngày 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> events= [[1,2],[2,3],[3,4],[1,2]]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= events.length &lt;= 10<sup>5</sup></code></li>
	<li><code>events[i].length == 2</code></li>
	<li><code>1 &lt;= startDay<sub>i</sub> &lt;= endDay<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Greedy + Priority Queue

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày chỉ tham dự được một sự kiện, và sự kiện có thể được tham dự vào bất kỳ ngày nào trong $[s,e]$. $n$ và khoảng ngày đều có thể lên đến $10^5$, nên sắp xếp theo ngày kết thúc rồi duyệt ngày một cách trực tiếp sẽ không hiệu quả. Duyệt lần lượt từng ngày: đưa các sự kiện bắt đầu hôm đó vào min-heap, loại bỏ sự kiện đã hết hạn, rồi tham dự sự kiện có ngày kết thúc sớm nhất. Như vậy, mỗi ngày được dành cho sự kiện cấp thiết nhất trong số các sự kiện còn lại.

<!-- thinking:end -->

Ta dùng hash table $\textit{g}$ để lưu thời điểm bắt đầu và kết thúc của các sự kiện. Key là ngày bắt đầu, value là danh sách ngày kết thúc của tất cả sự kiện bắt đầu vào ngày đó. Hai biến $\textit{l}$ và $\textit{r}$ lần lượt lưu ngày bắt đầu nhỏ nhất và ngày kết thúc lớn nhất trong các sự kiện.

Với mỗi ngày $s$ từ $\textit{l}$ đến $\textit{r}$ theo thứ tự tăng dần, ta thực hiện:

1. Loại khỏi priority queue mọi sự kiện có ngày kết thúc nhỏ hơn ngày hiện tại $s$.
2. Thêm ngày kết thúc của mọi sự kiện bắt đầu vào ngày hiện tại $s$ vào priority queue.
3. Nếu priority queue không rỗng, lấy sự kiện có ngày kết thúc sớm nhất, tăng đáp án lên một và xóa sự kiện đó khỏi priority queue.

Nhờ vậy, vào mỗi ngày $s$, ta luôn tham dự sự kiện kết thúc sớm nhất, từ đó tối đa hóa số sự kiện tham dự.

Độ phức tạp thời gian là $O(M \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $M$ là ngày kết thúc lớn nhất và $n$ là số sự kiện.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxEvents(self, events: List[List[int]]) -> int:
        g = defaultdict(list)
        l, r = inf, 0
        for s, e in events:
            g[s].append(e)
            l = min(l, s)
            r = max(r, e)
        pq = []
        ans = 0
        for s in range(l, r + 1):
            while pq and pq[0] < s:
                heappop(pq)
            for e in g[s]:
                heappush(pq, e)
            if pq:
                heappop(pq)
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxEvents(int[][] events) {
        Map<Integer, List<Integer>> g = new HashMap<>();
        int l = Integer.MAX_VALUE, r = 0;
        for (int[] event : events) {
            int s = event[0], e = event[1];
            g.computeIfAbsent(s, k -> new ArrayList<>()).add(e);
            l = Math.min(l, s);
            r = Math.max(r, e);
        }
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        int ans = 0;
        for (int s = l; s <= r; s++) {
            while (!pq.isEmpty() && pq.peek() < s) {
                pq.poll();
            }
            for (int e : g.getOrDefault(s, List.of())) {
                pq.offer(e);
            }
            if (!pq.isEmpty()) {
                pq.poll();
                ans++;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxEvents(vector<vector<int>>& events) {
        unordered_map<int, vector<int>> g;
        int l = INT_MAX, r = 0;
        for (auto& event : events) {
            int s = event[0], e = event[1];
            g[s].push_back(e);
            l = min(l, s);
            r = max(r, e);
        }
        priority_queue<int, vector<int>, greater<int>> pq;
        int ans = 0;
        for (int s = l; s <= r; ++s) {
            while (!pq.empty() && pq.top() < s) {
                pq.pop();
            }
            for (int e : g[s]) {
                pq.push(e);
            }
            if (!pq.empty()) {
                pq.pop();
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxEvents(events [][]int) (ans int) {
	g := map[int][]int{}
	l, r := math.MaxInt32, 0
	for _, event := range events {
		s, e := event[0], event[1]
		g[s] = append(g[s], e)
		l = min(l, s)
		r = max(r, e)
	}

	pq := &hp{}
	heap.Init(pq)
	for s := l; s <= r; s++ {
		for pq.Len() > 0 && pq.IntSlice[0] < s {
			heap.Pop(pq)
		}
		for _, e := range g[s] {
			heap.Push(pq, e)
		}
		if pq.Len() > 0 {
			heap.Pop(pq)
			ans++
		}
	}
	return
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	n := len(h.IntSlice)
	v := h.IntSlice[n-1]
	h.IntSlice = h.IntSlice[:n-1]
	return v
}
func (h *hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
```

#### TypeScript

```ts
function maxEvents(events: number[][]): number {
    const g: Map<number, number[]> = new Map();
    let l = Infinity,
        r = 0;
    for (const [s, e] of events) {
        if (!g.has(s)) g.set(s, []);
        g.get(s)!.push(e);
        l = Math.min(l, s);
        r = Math.max(r, e);
    }

    const pq = new MinPriorityQueue<number>();
    let ans = 0;
    for (let s = l; s <= r; s++) {
        while (!pq.isEmpty() && pq.front() < s) {
            pq.dequeue();
        }
        for (const e of g.get(s) || []) {
            pq.enqueue(e);
        }
        if (!pq.isEmpty()) {
            pq.dequeue();
            ans++;
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::{BinaryHeap, HashMap};
use std::cmp::Reverse;

impl Solution {
    pub fn max_events(events: Vec<Vec<i32>>) -> i32 {
        let mut g: HashMap<i32, Vec<i32>> = HashMap::new();
        let mut l = i32::MAX;
        let mut r = 0;

        for event in events {
            let s = event[0];
            let e = event[1];
            g.entry(s).or_default().push(e);
            l = l.min(s);
            r = r.max(e);
        }

        let mut pq = BinaryHeap::new();
        let mut ans = 0;

        for s in l..=r {
            while let Some(&Reverse(top)) = pq.peek() {
                if top < s {
                    pq.pop();
                } else {
                    break;
                }
            }
            if let Some(ends) = g.get(&s) {
                for &e in ends {
                    pq.push(Reverse(e));
                }
            }
            if pq.pop().is_some() {
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
