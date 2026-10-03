---
comments: true
difficulty: Medium
rating: 1695
source: Biweekly Contest 57 Q2
tags:
    - Array
    - Hash Table
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1942. The Number of the Smallest Unoccupied Chair](https://leetcode.com/problems/the-number-of-the-smallest-unoccupied-chair)

[中文文档](/solution/1900-1999/1942.The%20Number%20of%20the%20Smallest%20Unoccupied%20Chair/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bữa tiệc với <code>n</code> người bạn được đánh số từ <code>0</code> đến <code>n - 1</code> tham dự. Có <strong>vô hạn</strong> ghế trong bữa tiệc, được đánh số từ <code>0</code> đến <code>infinity</code>. Khi một người bạn đến bữa tiệc, họ sẽ ngồi vào chiếc ghế trống có <strong>số nhỏ nhất</strong>.</p>

<ul>
	<li>Ví dụ, nếu các ghế <code>0</code>, <code>1</code> và <code>5</code> đang có người ngồi khi một người bạn đến, họ sẽ ngồi vào ghế số <code>2</code>.</li>
</ul>

<p>Khi một người bạn rời bữa tiệc, chiếc ghế của họ sẽ trở thành ghế trống ngay tại thời điểm họ rời đi. Nếu một người bạn khác đến vào đúng thời điểm đó, họ có thể ngồi vào chiếc ghế này.</p>

<p>Bạn được cung cấp một mảng số nguyên 2 chiều <strong>đánh chỉ số từ 0</strong> <code>times</code>, trong đó <code>times[i] = [arrival<sub>i</sub>, leaving<sub>i</sub>]</code> lần lượt biểu thị thời điểm đến và rời đi của người bạn thứ <code>i<sup>th</sup></code>, cùng với một số nguyên <code>targetFriend</code>. Tất cả thời điểm đến đều <strong>khác nhau</strong>.</p>

<p>Hãy trả về <em><strong>số ghế</strong> mà người bạn có số </em><code>targetFriend</code><em> sẽ ngồi vào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> times = [[1,4],[2,3],[4,6]], targetFriend = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
- Người bạn 0 đến vào thời điểm 1 và ngồi vào ghế 0.
- Người bạn 1 đến vào thời điểm 2 và ngồi vào ghế 1.
- Người bạn 1 rời đi vào thời điểm 3, ghế 1 trở thành ghế trống.
- Người bạn 0 rời đi vào thời điểm 4, ghế 0 trở thành ghế trống.
- Người bạn 2 đến vào thời điểm 4 và ngồi vào ghế 0.
Vì người bạn 1 ngồi vào ghế 1, ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> times = [[3,10],[1,5],[2,6]], targetFriend = 0
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Người bạn 1 đến vào thời điểm 1 và ngồi vào ghế 0.
- Người bạn 2 đến vào thời điểm 2 và ngồi vào ghế 1.
- Người bạn 0 đến vào thời điểm 3 và ngồi vào ghế 2.
- Người bạn 1 rời đi vào thời điểm 5, ghế 0 trở thành ghế trống.
- Người bạn 2 rời đi vào thời điểm 6, ghế 1 trở thành ghế trống.
- Người bạn 0 rời đi vào thời điểm 10, ghế 2 trở thành ghế trống.
Vì người bạn 0 ngồi vào ghế 2, ta trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == times.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>times[i].length == 2</code></li>
	<li><code>1 &lt;= arrival<sub>i</sub> &lt; leaving<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= targetFriend &lt;= n - 1</code></li>
	<li>Thời điểm <code>arrival<sub>i</sub></code> của mỗi người bạn đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người bạn chọn chiếc ghế trống nhỏ nhất khi đến và trả lại ghế khi rời đi. Nếu duyệt qua tất cả các ghế mỗi lần, ta sẽ phát sinh thêm một thừa số tuyến tính.
>
> Một min-heap chứa chỉ số các ghế trống và một min-heap chứa $(\textit{leaving},\textit{chair})$ sẽ xử lý việc tái sử dụng ghế. Khi có người đến, ta giải phóng mọi ghế đã đến hạn, sau đó lấy chỉ số ghế trống nhỏ nhất.
>
> Ta dừng lại khi người bạn mục tiêu đã được xếp chỗ. Việc sắp xếp cùng với các heap có độ phức tạp $O(n\log n)$.

<!-- thinking:end -->

Trước tiên, ta tạo một tuple cho mỗi người bạn, gồm thời điểm đến, thời điểm rời đi và chỉ số của họ, sau đó sắp xếp các tuple này theo thời điểm đến.

Ta sử dụng một min-heap $\textit{idle}$ để lưu số hiệu các ghế hiện đang trống. Ban đầu, ta thêm $0, 1, \ldots, n-1$ vào $\textit{idle}$. Ta cũng sử dụng một min-heap $\textit{busy}$ để lưu các tuple $(\textit{leaving}, \textit{chair})$, trong đó $\textit{leaving}$ là thời điểm rời đi và $\textit{chair}$ là số hiệu ghế.

Ta duyệt qua thời điểm đến, thời điểm rời đi và chỉ số của từng người bạn. Với mỗi người, trước tiên ta loại khỏi $\textit{busy}$ tất cả những người có thời điểm rời đi nhỏ hơn hoặc bằng thời điểm đến hiện tại, đồng thời đưa số hiệu ghế của họ trở lại $\textit{idle}$. Sau đó, ta lấy một số hiệu ghế từ $\textit{idle}$, gán cho người bạn hiện tại và thêm $(\textit{leaving}, \textit{chair})$ vào $\textit{busy}$. Nếu chỉ số của người bạn hiện tại bằng $\textit{targetFriend}$, ta trả về số hiệu ghế được gán.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số người bạn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestChair(self, times: List[List[int]], targetFriend: int) -> int:
        n = len(times)
        for i in range(n):
            times[i].append(i)
        times.sort()
        idle = list(range(n))
        heapify(idle)
        busy = []
        for arrival, leaving, i in times:
            while busy and busy[0][0] <= arrival:
                heappush(idle, heappop(busy)[1])
            j = heappop(idle)
            if i == targetFriend:
                return j
            heappush(busy, (leaving, j))
```

#### Java

```java
class Solution {
    public int smallestChair(int[][] times, int targetFriend) {
        int n = times.length;
        PriorityQueue<Integer> idle = new PriorityQueue<>();
        PriorityQueue<int[]> busy = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        for (int i = 0; i < n; ++i) {
            times[i] = new int[] {times[i][0], times[i][1], i};
            idle.offer(i);
        }
        Arrays.sort(times, Comparator.comparingInt(a -> a[0]));
        for (var e : times) {
            int arrival = e[0], leaving = e[1], i = e[2];
            while (!busy.isEmpty() && busy.peek()[0] <= arrival) {
                idle.offer(busy.poll()[1]);
            }
            int j = idle.poll();
            if (i == targetFriend) {
                return j;
            }
            busy.offer(new int[] {leaving, j});
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestChair(vector<vector<int>>& times, int targetFriend) {
        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> busy;
        priority_queue<int, vector<int>, greater<int>> idle;
        int n = times.size();
        for (int i = 0; i < n; ++i) {
            times[i].push_back(i);
            idle.push(i);
        }
        ranges::sort(times);
        for (const auto& e : times) {
            int arrival = e[0], leaving = e[1], i = e[2];
            while (!busy.empty() && busy.top().first <= arrival) {
                idle.push(busy.top().second);
                busy.pop();
            }
            int j = idle.top();
            if (i == targetFriend) {
                return j;
            }
            idle.pop();
            busy.emplace(leaving, j);
        }
        return -1;
    }
};
```

#### Go

```go
func smallestChair(times [][]int, targetFriend int) int {
	idle := hp{}
	busy := hp2{}
	for i := range times {
		times[i] = append(times[i], i)
		heap.Push(&idle, i)
	}
	sort.Slice(times, func(i, j int) bool { return times[i][0] < times[j][0] })
	for _, e := range times {
		arrival, leaving, i := e[0], e[1], e[2]
		for len(busy) > 0 && busy[0].t <= arrival {
			heap.Push(&idle, heap.Pop(&busy).(pair).i)
		}
		j := heap.Pop(&idle).(int)
		if i == targetFriend {
			return j
		}
		heap.Push(&busy, pair{leaving, j})
	}
	return -1
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}

type pair struct{ t, i int }
type hp2 []pair

func (h hp2) Len() int           { return len(h) }
func (h hp2) Less(i, j int) bool { return h[i].t < h[j].t || (h[i].t == h[j].t && h[i].i < h[j].i) }
func (h hp2) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp2) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp2) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function smallestChair(times: number[][], targetFriend: number): number {
    const n = times.length;
    const idle = new MinPriorityQueue<number>();
    const busy = new PriorityQueue((a, b) => a[0] - b[0]);
    for (let i = 0; i < n; ++i) {
        times[i].push(i);
        idle.enqueue(i);
    }
    times.sort((a, b) => a[0] - b[0]);
    for (const [arrival, leaving, i] of times) {
        while (busy.size() > 0 && busy.front()[0] <= arrival) {
            idle.enqueue(busy.dequeue()[1]);
        }
        const j = idle.dequeue();
        if (i === targetFriend) {
            return j;
        }
        busy.enqueue([leaving, j]);
    }
    return -1;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} times
 * @param {number} targetFriend
 * @return {number}
 */
var smallestChair = function (times, targetFriend) {
    const n = times.length;
    const idle = new MinPriorityQueue();
    const busy = new PriorityQueue((a, b) => a[0] - b[0]);
    for (let i = 0; i < n; ++i) {
        times[i].push(i);
        idle.enqueue(i);
    }
    times.sort((a, b) => a[0] - b[0]);
    for (const [arrival, leaving, i] of times) {
        while (busy.size() > 0 && busy.front()[0] <= arrival) {
            idle.enqueue(busy.dequeue()[1]);
        }
        const j = idle.dequeue();
        if (i === targetFriend) {
            return j;
        }
        busy.enqueue([leaving, j]);
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
