---
comments: true
difficulty: Hard
rating: 2286
source: Weekly Contest 239 Q4
tags:
    - Array
    - Binary Search
    - Sorting
    - Sweep Line
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1851. Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query)

[中文文档](/solution/1800-1899/1851.Minimum%20Interval%20to%20Include%20Each%20Query/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>intervals</code>, trong đó <code>intervals[i] = [left<sub>i</sub>, right<sub>i</sub>]</code> mô tả đoạn thứ <code>i<sup>th</sup></code>, bắt đầu tại <code>left<sub>i</sub></code> và kết thúc tại <code>right<sub>i</sub></code> <strong>(bao gồm cả hai đầu mút)</strong>. <strong>Độ dài</strong> của một đoạn được định nghĩa là số lượng số nguyên mà nó chứa, hay chính xác hơn là <code>right<sub>i</sub> - left<sub>i</sub> + 1</code>.</p>

<p>Ta cũng được cho một mảng số nguyên <code>queries</code>. Đáp án cho query thứ <code>j<sup>th</sup></code> là <strong>độ dài của đoạn nhỏ nhất</strong> <code>i</code> sao cho <code>left<sub>i</sub> &lt;= queries[j] &lt;= right<sub>i</sub></code>. Nếu không tồn tại đoạn như vậy, đáp án là <code>-1</code>.</p>

<p>Trả về <em>một mảng chứa đáp án cho các query</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
<strong>Đầu ra:</strong> [3,3,1,4]
<strong>Giải thích:</strong> Các query được xử lý như sau:
- Query = 2: Đoạn [2,4] là đoạn nhỏ nhất chứa 2. Đáp án là 4 - 2 + 1 = 3.
- Query = 3: Đoạn [2,4] là đoạn nhỏ nhất chứa 3. Đáp án là 4 - 2 + 1 = 3.
- Query = 4: Đoạn [4,4] là đoạn nhỏ nhất chứa 4. Đáp án là 4 - 4 + 1 = 1.
- Query = 5: Đoạn [3,6] là đoạn nhỏ nhất chứa 5. Đáp án là 6 - 3 + 1 = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
<strong>Đầu ra:</strong> [2,-1,4,6]
<strong>Giải thích:</strong> Các query được xử lý như sau:
- Query = 2: Đoạn [2,3] là đoạn nhỏ nhất chứa 2. Đáp án là 3 - 2 + 1 = 2.
- Query = 19: Không có đoạn nào chứa 19. Đáp án là -1.
- Query = 5: Đoạn [2,5] là đoạn nhỏ nhất chứa 5. Đáp án là 5 - 2 + 1 = 4.
- Query = 22: Đoạn [20,25] là đoạn nhỏ nhất chứa 22. Đáp án là 25 - 20 + 1 = 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= intervals.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>1 &lt;= left<sub>i</sub> &lt;= right<sub>i</sub> &lt;= 10<sup>7</sup></code></li>
	<li><code>1 &lt;= queries[j] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Xử lý query offline + Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi query cần đoạn ngắn nhất chứa điểm đó. Cả hai mảng có thể có kích thước $10^5$, nên không thể duyệt toàn bộ các đoạn cho từng query.
>
> Sắp xếp các query offline theo tọa độ và các đoạn theo đầu trái. Một min-heap lưu $(\textit{length},\textit{right})$ của các đoạn đã bắt đầu: thêm những đoạn có đầu trái không lớn hơn query, loại những đoạn có đầu phải quá nhỏ. Phần tử đầu heap là đoạn ngắn nhất chứa điểm hiện tại.

<!-- thinking:end -->

Ta nhận thấy thứ tự của các query không ảnh hưởng đến đáp án, và các đoạn liên quan cũng không thay đổi. Vì vậy, ta sắp xếp tất cả query theo thứ tự tăng dần, đồng thời sắp xếp tất cả đoạn theo đầu trái tăng dần.

Ta dùng một priority queue (min heap) $pq$ để duy trì tất cả các đoạn hiện tại. Mỗi phần tử trong queue là một cặp $(v, r)$, biểu diễn một đoạn có độ dài $v$ và đầu phải $r$. Ban đầu priority queue rỗng. Ngoài ra, ta định nghĩa một con trỏ $i$ trỏ tới đoạn hiện tại đang được duyệt, ban đầu $i=0$.

Ta duyệt từng query $(x, j)$ theo thứ tự tăng dần và thực hiện các thao tác sau:

- Nếu con trỏ $i$ chưa duyệt hết các đoạn và đầu trái của đoạn hiện tại $[a, b]$ nhỏ hơn hoặc bằng $x$, ta thêm đoạn này vào priority queue rồi tăng con trỏ $i$ lên một bước. Lặp lại thao tác này.
- Nếu priority queue không rỗng và đầu phải của phần tử đầu heap nhỏ hơn $x$, ta loại phần tử đầu heap. Lặp lại thao tác này.
- Lúc này, nếu priority queue không rỗng thì phần tử đầu heap là đoạn nhỏ nhất chứa $x$. Ta thêm độ dài $v$ của nó vào mảng đáp án $ans$.

Sau khi hoàn tất các thao tác trên, ta trả về mảng đáp án $ans$.

Độ phức tạp thời gian là $O(n \times \log n + m \times \log m)$, độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của các mảng `intervals` và `queries`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minInterval(self, intervals: List[List[int]], queries: List[int]) -> List[int]:
        n, m = len(intervals), len(queries)
        intervals.sort()
        queries = sorted((x, i) for i, x in enumerate(queries))
        ans = [-1] * m
        pq = []
        i = 0
        for x, j in queries:
            while i < n and intervals[i][0] <= x:
                a, b = intervals[i]
                heappush(pq, (b - a + 1, b))
                i += 1
            while pq and pq[0][1] < x:
                heappop(pq)
            if pq:
                ans[j] = pq[0][0]
        return ans
```

#### Java

```java
class Solution {
    public int[] minInterval(int[][] intervals, int[] queries) {
        int n = intervals.length, m = queries.length;
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int[][] qs = new int[m][0];
        for (int i = 0; i < m; ++i) {
            qs[i] = new int[] {queries[i], i};
        }
        Arrays.sort(qs, (a, b) -> a[0] - b[0]);
        int[] ans = new int[m];
        Arrays.fill(ans, -1);
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        int i = 0;
        for (int[] q : qs) {
            while (i < n && intervals[i][0] <= q[0]) {
                int a = intervals[i][0], b = intervals[i][1];
                pq.offer(new int[] {b - a + 1, b});
                ++i;
            }
            while (!pq.isEmpty() && pq.peek()[1] < q[0]) {
                pq.poll();
            }
            if (!pq.isEmpty()) {
                ans[q[1]] = pq.peek()[0];
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
    vector<int> minInterval(vector<vector<int>>& intervals, vector<int>& queries) {
        int n = intervals.size(), m = queries.size();
        sort(intervals.begin(), intervals.end());
        using pii = pair<int, int>;
        vector<pii> qs;
        for (int i = 0; i < m; ++i) {
            qs.emplace_back(queries[i], i);
        }
        sort(qs.begin(), qs.end());
        vector<int> ans(m, -1);
        priority_queue<pii, vector<pii>, greater<pii>> pq;
        int i = 0;
        for (auto& [x, j] : qs) {
            while (i < n && intervals[i][0] <= x) {
                int a = intervals[i][0], b = intervals[i][1];
                pq.emplace(b - a + 1, b);
                ++i;
            }
            while (!pq.empty() && pq.top().second < x) {
                pq.pop();
            }
            if (!pq.empty()) {
                ans[j] = pq.top().first;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minInterval(intervals [][]int, queries []int) []int {
	n, m := len(intervals), len(queries)
	sort.Slice(intervals, func(i, j int) bool { return intervals[i][0] < intervals[j][0] })
	qs := make([][2]int, m)
	ans := make([]int, m)
	for i := range qs {
		qs[i] = [2]int{queries[i], i}
		ans[i] = -1
	}
	sort.Slice(qs, func(i, j int) bool { return qs[i][0] < qs[j][0] })
	pq := hp{}
	i := 0
	for _, q := range qs {
		x, j := q[0], q[1]
		for i < n && intervals[i][0] <= x {
			a, b := intervals[i][0], intervals[i][1]
			heap.Push(&pq, pair{b - a + 1, b})
			i++
		}
		for len(pq) > 0 && pq[0].r < x {
			heap.Pop(&pq)
		}
		if len(pq) > 0 {
			ans[j] = pq[0].v
		}
	}
	return ans
}

type pair struct{ v, r int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].v < h[j].v }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function minInterval(intervals: number[][], queries: number[]): number[] {
    const n = intervals.length;
    const m = queries.length;

    intervals.sort((a, b) => a[0] - b[0]);
    const qs = queries.map((x, i) => [x, i] as [number, number]).sort((a, b) => a[0] - b[0]);

    const ans = Array<number>(m).fill(-1);
    const pq = new PriorityQueue<[number, number]>((a, b) =>
        a[0] === b[0] ? a[1] - b[1] : a[0] - b[0],
    );

    let i = 0;
    for (const [x, idx] of qs) {
        while (i < n && intervals[i][0] <= x) {
            const [l, r] = intervals[i];
            pq.enqueue([r - l + 1, r]);
            i++;
        }

        while (pq.size() > 0 && pq.front()![1] < x) {
            pq.dequeue();
        }

        if (pq.size() > 0) {
            ans[idx] = pq.front()![0];
        }
    }

    return ans;
}
```

#### Rust

```rust
use std::cmp::Reverse;
use std::collections::BinaryHeap;

impl Solution {
    pub fn min_interval(intervals: Vec<Vec<i32>>, queries: Vec<i32>) -> Vec<i32> {
        let mut intervals = intervals;
        intervals.sort_by_key(|v| v[0]);

        let mut sorted_queries: Vec<(i32, usize)> = queries
            .into_iter()
            .enumerate()
            .map(|(i, x)| (x, i))
            .collect();
        sorted_queries.sort_by_key(|&(x, _)| x);

        let mut ans = vec![-1; sorted_queries.len()];
        let mut heap: BinaryHeap<Reverse<(i32, i32)>> = BinaryHeap::new();
        let mut i = 0usize;

        for (x, idx) in sorted_queries {
            while i < intervals.len() && intervals[i][0] <= x {
                let l = intervals[i][0];
                let r = intervals[i][1];
                heap.push(Reverse((r - l + 1, r)));
                i += 1;
            }

            while let Some(&Reverse((_, r))) = heap.peek() {
                if r < x {
                    heap.pop();
                } else {
                    break;
                }
            }

            if let Some(&Reverse((len, _))) = heap.peek() {
                ans[idx] = len;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
