---
comments: true
difficulty: Hard
rating: 2092
source: Weekly Contest 309 Q4
tags:
    - Array
    - Hash Table
    - Sorting
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2402. Meeting Rooms III](https://leetcode.com/problems/meeting-rooms-iii)

[中文文档](/solution/2400-2499/2402.Meeting%20Rooms%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>. Có <code>n</code> phòng được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Cho một mảng số nguyên 2 chiều <code>meetings</code>, trong đó <code>meetings[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> nghĩa là một cuộc họp sẽ diễn ra trong khoảng thời gian <strong>nửa kín</strong> <code>[start<sub>i</sub>, end<sub>i</sub>)</code>. Tất cả các giá trị của <code>start<sub>i</sub></code> đều <strong>khác nhau</strong>.</p>

<p>Các cuộc họp được phân bổ vào các phòng theo cách sau:</p>

<ol>
	<li>Mỗi cuộc họp sẽ diễn ra trong phòng chưa được sử dụng có số hiệu <strong>nhỏ nhất</strong>.</li>
	<li>Nếu không còn phòng trống, cuộc họp sẽ bị trì hoãn cho đến khi một phòng được giải phóng. Cuộc họp bị trì hoãn phải có <strong>cùng</strong> thời lượng với cuộc họp ban đầu.</li>
	<li>Khi một phòng được giải phóng, cuộc họp có thời điểm <strong>bắt đầu</strong> ban đầu sớm hơn sẽ được ưu tiên sử dụng phòng đó.</li>
</ol>

<p>Trả về <em><strong>số hiệu</strong> của phòng đã tổ chức nhiều cuộc họp nhất</em>. Nếu có nhiều phòng như vậy, trả về <em>phòng có số hiệu <strong>nhỏ nhất</strong></em>.</p>

<p><strong>Khoảng thời gian nửa kín</strong> <code>[a, b)</code> là khoảng giữa <code>a</code> và <code>b</code>, <strong>bao gồm</strong> <code>a</code> và <strong>không bao gồm</strong> <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, meetings = [[0,10],[1,5],[2,7],[3,4]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
- Tại thời điểm 0, cả hai phòng đều chưa được sử dụng. Cuộc họp đầu tiên bắt đầu trong phòng 0.
- Tại thời điểm 1, chỉ phòng 1 chưa được sử dụng. Cuộc họp thứ hai bắt đầu trong phòng 1.
- Tại thời điểm 2, cả hai phòng đều đang được sử dụng. Cuộc họp thứ ba bị trì hoãn.
- Tại thời điểm 3, cả hai phòng đều đang được sử dụng. Cuộc họp thứ tư bị trì hoãn.
- Tại thời điểm 5, cuộc họp trong phòng 1 kết thúc. Cuộc họp thứ ba bắt đầu trong phòng 1 trong khoảng thời gian [5,10).
- Tại thời điểm 10, các cuộc họp trong cả hai phòng đều kết thúc. Cuộc họp thứ tư bắt đầu trong phòng 0 trong khoảng thời gian [10,11).
Cả hai phòng 0 và 1 đều đã tổ chức 2 cuộc họp, nên ta trả về 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, meetings = [[1,20],[2,10],[3,5],[4,9],[6,8]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
- Tại thời điểm 1, cả ba phòng đều chưa được sử dụng. Cuộc họp đầu tiên bắt đầu trong phòng 0.
- Tại thời điểm 2, các phòng 1 và 2 chưa được sử dụng. Cuộc họp thứ hai bắt đầu trong phòng 1.
- Tại thời điểm 3, chỉ phòng 2 chưa được sử dụng. Cuộc họp thứ ba bắt đầu trong phòng 2.
- Tại thời điểm 4, cả ba phòng đều đang được sử dụng. Cuộc họp thứ tư bị trì hoãn.
- Tại thời điểm 5, cuộc họp trong phòng 2 kết thúc. Cuộc họp thứ tư bắt đầu trong phòng 2 trong khoảng thời gian [5,10).
- Tại thời điểm 6, cả ba phòng đều đang được sử dụng. Cuộc họp thứ năm bị trì hoãn.
- Tại thời điểm 10, các cuộc họp trong phòng 1 và 2 kết thúc. Cuộc họp thứ năm bắt đầu trong phòng 1 trong khoảng thời gian [10,12).
Phòng 0 đã tổ chức 1 cuộc họp, trong khi các phòng 1 và 2 đều đã tổ chức 2 cuộc họp, nên ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= meetings.length &lt;= 10<sup>5</sup></code></li>
	<li><code>meetings[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 5 * 10<sup>5</sup></code></li>
	<li>Tất cả các giá trị của <code>start<sub>i</sub></code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 100$ và $m\le 10^5$, việc duyệt qua mọi phòng cho mỗi cuộc họp có độ phức tạp $O(mn)$ và gần chạm giới hạn. Các thời điểm bắt đầu khác nhau, nên các cuộc họp phải được phân bổ theo thứ tự thời gian.
>
> Các phòng trống được chọn theo chỉ số nhỏ nhất; các phòng đang bận được giải phóng theo thời điểm kết thúc sớm nhất. Hai heap sẽ duy trì hai nhóm này. Sau khi sắp xếp các cuộc họp theo thời điểm bắt đầu, đưa các phòng đã kết thúc vào heap phòng trống; nếu có phòng trống thì lấy phòng có chỉ số nhỏ nhất, nếu không thì trì hoãn cuộc họp vào phòng kết thúc sớm nhất trong khoảng thời gian bằng thời lượng cuộc họp.

<!-- thinking:end -->

Ta định nghĩa hai priority queue lần lượt biểu diễn các phòng họp đang trống và đang bận. Các phòng đang trống $\textit{idle}$ được sắp xếp theo **chỉ số**; các phòng đang bận $\textit{busy}$ được sắp xếp theo **thời điểm kết thúc và chỉ số**.

Đầu tiên, sắp xếp các cuộc họp theo thời điểm bắt đầu, sau đó duyệt qua các cuộc họp. Với mỗi cuộc họp:

- Nếu có các phòng đang bận với thời điểm kết thúc nhỏ hơn hoặc bằng thời điểm bắt đầu của cuộc họp hiện tại, thêm chúng vào queue các phòng trống $\textit{idle}$;
- Nếu còn phòng trống, lấy phòng có chỉ số nhỏ nhất từ queue $\textit{idle}$ và thêm phòng đó vào queue các phòng đang bận $\textit{busy}$;
- Nếu không còn phòng trống, tìm phòng có thời điểm kết thúc sớm nhất và chỉ số nhỏ nhất trong queue $\textit{busy}$, rồi thêm phòng đó trở lại queue $\textit{busy}$.

Độ phức tạp thời gian là $O(m (\log m + \log n))$, độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số phòng họp và số cuộc họp.

Các bài tương tự:

- [1882. Process Tasks Using Servers](https://github.com/doocs/leetcode/blob/main/solution/1800-1899/1882.Process%20Tasks%20Using%20Servers/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostBooked(self, n: int, meetings: List[List[int]]) -> int:
        meetings.sort(key=lambda x: x[0])
        busy = []
        idle = list(range(n))
        heapify(idle)
        cnt = [0] * n
        for s, e in meetings:
            while busy and busy[0][0] <= s:
                heappush(idle, heappop(busy)[1])
            i = 0
            if idle:
                i = heappop(idle)
                heappush(busy, (e, i))
            else:
                time_end, i = heappop(busy)
                heappush(busy, (time_end + e - s, i))
            cnt[i] += 1
        ans = 0
        for i in range(n):
            if cnt[ans] < cnt[i]:
                ans = i
        return ans
```

#### Java

```java
class Solution {
    public int mostBooked(int n, int[][] meetings) {
        Arrays.sort(meetings, (a, b) -> a[0] - b[0]);
        PriorityQueue<int[]> busy
            = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        PriorityQueue<Integer> idle = new PriorityQueue<>();
        for (int i = 0; i < n; ++i) {
            idle.offer(i);
        }
        int[] cnt = new int[n];
        for (var v : meetings) {
            int s = v[0], e = v[1];
            while (!busy.isEmpty() && busy.peek()[0] <= s) {
                idle.offer(busy.poll()[1]);
            }
            int i = 0;
            if (!idle.isEmpty()) {
                i = idle.poll();
                busy.offer(new int[] {e, i});
            } else {
                var x = busy.poll();
                i = x[1];
                busy.offer(new int[] {x[0] + e - s, i});
            }
            ++cnt[i];
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (cnt[ans] < cnt[i]) {
                ans = i;
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
    int mostBooked(int n, vector<vector<int>>& meetings) {
        sort(meetings.begin(), meetings.end());
        using pli = pair<long long, int>;
        priority_queue<pli, vector<pli>, greater<pli>> busy;
        priority_queue<int, vector<int>, greater<int>> idle;
        for (int i = 0; i < n; ++i) {
            idle.push(i);
        }
        vector<int> cnt(n);
        for (auto& v : meetings) {
            int s = v[0], e = v[1];
            while (!busy.empty() && busy.top().first <= s) {
                idle.push(busy.top().second);
                busy.pop();
            }
            int i = 0;
            if (!idle.empty()) {
                i = idle.top();
                idle.pop();
                busy.push({e, i});
            } else {
                auto x = busy.top();
                busy.pop();
                i = x.second;
                busy.push({x.first + e - s, i});
            }
            ++cnt[i];
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (cnt[ans] < cnt[i]) {
                ans = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func mostBooked(n int, meetings [][]int) int {
	sort.Slice(meetings, func(i, j int) bool { return meetings[i][0] < meetings[j][0] })
	idle := hp{make([]int, n)}
	for i := 0; i < n; i++ {
		idle.IntSlice[i] = i
	}
	busy := hp2{}
	cnt := make([]int, n)
	for _, v := range meetings {
		s, e := v[0], v[1]
		for len(busy) > 0 && busy[0].end <= s {
			heap.Push(&idle, heap.Pop(&busy).(pair).i)
		}
		var i int
		if idle.Len() > 0 {
			i = heap.Pop(&idle).(int)
			heap.Push(&busy, pair{e, i})
		} else {
			x := heap.Pop(&busy).(pair)
			i = x.i
			heap.Push(&busy, pair{x.end + e - s, i})
		}
		cnt[i]++
	}
	ans := 0
	for i, v := range cnt {
		if cnt[ans] < v {
			ans = i
		}
	}
	return ans
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}

type pair struct{ end, i int }
type hp2 []pair

func (h hp2) Len() int { return len(h) }
func (h hp2) Less(i, j int) bool {
	a, b := h[i], h[j]
	return a.end < b.end || a.end == b.end && a.i < b.i
}
func (h hp2) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp2) Push(v any)   { *h = append(*h, v.(pair)) }
func (h *hp2) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function mostBooked(n: number, meetings: number[][]): number {
    meetings.sort((a, b) => a[0] - b[0]);

    const idle = new MinPriorityQueue<number>();
    for (let i = 0; i < n; ++i) {
        idle.enqueue(i);
    }
    const busy = new PriorityQueue<[number, number]>((a, b) => {
        if (a[0] === b[0]) {
            return a[1] - b[1];
        }
        return a[0] - b[0];
    });
    const cnt: number[] = new Array(n).fill(0);
    for (const v of meetings) {
        const s = v[0],
            e = v[1];
        while (!busy.isEmpty() && busy.front()[0] <= s) {
            const i = busy.dequeue()[1];
            idle.enqueue(i);
        }
        let i = 0;
        if (!idle.isEmpty()) {
            i = idle.dequeue();
            busy.enqueue([e, i]);
        } else {
            const x = busy.dequeue();
            i = x[1];
            busy.enqueue([x[0] + e - s, i]);
        }
        ++cnt[i];
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (cnt[ans] < cnt[i]) {
            ans = i;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
