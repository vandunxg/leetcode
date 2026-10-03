---
comments: true
difficulty: Medium
rating: 1797
source: Weekly Contest 237 Q3
tags:
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1834. Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu)

[中文文档](/solution/1800-1899/1834.Single-Threaded%20CPU/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code>​​​​​​ tác vụ được đánh số từ <code>0</code> đến <code>n - 1</code>, biểu diễn bằng mảng số nguyên 2 chiều <code>tasks</code>, trong đó <code>tasks[i] = [enqueueTime<sub>i</sub>, processingTime<sub>i</sub>]</code> có nghĩa là tác vụ thứ <code>i<sup>​​​​​​th</sup></code>​​​​ có thể được xử lý tại thời điểm <code>enqueueTime<sub>i</sub></code> và cần <code>processingTime<sub>i</sub></code><sub> </sub>thời gian để hoàn tất.</p>

<p>Bạn có một CPU đơn luồng, chỉ có thể xử lý <strong>nhiều nhất một</strong> tác vụ tại một thời điểm và hoạt động như sau:</p>

<ul>
	<li>Nếu CPU đang rảnh và không có tác vụ nào sẵn sàng để xử lý, CPU tiếp tục ở trạng thái rảnh.</li>
	<li>Nếu CPU đang rảnh và có các tác vụ sẵn sàng, CPU sẽ chọn tác vụ có <strong>thời gian xử lý ngắn nhất</strong>. Nếu nhiều tác vụ có cùng thời gian xử lý ngắn nhất, CPU chọn tác vụ có chỉ số nhỏ nhất.</li>
	<li>Một khi đã bắt đầu một tác vụ, CPU sẽ <strong>xử lý toàn bộ tác vụ</strong> mà không dừng lại.</li>
	<li>CPU có thể hoàn tất một tác vụ rồi lập tức bắt đầu tác vụ mới.</li>
</ul>

<p>Trả về thứ tự CPU xử lý các tác vụ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [[1,2],[2,4],[3,2],[4,1]]
<strong>Đầu ra:</strong> [0,2,3,1]
<strong>Giải thích: </strong>Các sự kiện diễn ra như sau:
- Tại thời điểm = 1, tác vụ 0 có thể được xử lý. Các tác vụ sẵn sàng = {0}.
- Cũng tại thời điểm = 1, CPU đang rảnh bắt đầu xử lý tác vụ 0. Các tác vụ sẵn sàng = {}.
- Tại thời điểm = 2, tác vụ 1 có thể được xử lý. Các tác vụ sẵn sàng = {1}.
- Tại thời điểm = 3, tác vụ 2 có thể được xử lý. Các tác vụ sẵn sàng = {1, 2}.
- Cũng tại thời điểm = 3, CPU hoàn tất tác vụ 0 và bắt đầu xử lý tác vụ 2 vì đây là tác vụ ngắn nhất. Các tác vụ sẵn sàng = {1}.
- Tại thời điểm = 4, tác vụ 3 có thể được xử lý. Các tác vụ sẵn sàng = {1, 3}.
- Tại thời điểm = 5, CPU hoàn tất tác vụ 2 và bắt đầu xử lý tác vụ 3 vì đây là tác vụ ngắn nhất. Các tác vụ sẵn sàng = {1}.
- Tại thời điểm = 6, CPU hoàn tất tác vụ 3 và bắt đầu xử lý tác vụ 1. Các tác vụ sẵn sàng = {}.
- Tại thời điểm = 10, CPU hoàn tất tác vụ 1 và trở nên rảnh.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [[7,10],[7,12],[7,5],[7,4],[7,2]]
<strong>Đầu ra:</strong> [4,3,2,0,1]
<strong>Giải thích</strong><strong>: </strong>Các sự kiện diễn ra như sau:
- Tại thời điểm = 7, tất cả tác vụ đều có thể được xử lý. Các tác vụ sẵn sàng = {0,1,2,3,4}.
- Cũng tại thời điểm = 7, CPU đang rảnh bắt đầu xử lý tác vụ 4. Các tác vụ sẵn sàng = {0,1,2,3}.
- Tại thời điểm = 9, CPU hoàn tất tác vụ 4 và bắt đầu xử lý tác vụ 3. Các tác vụ sẵn sàng = {0,1,2}.
- Tại thời điểm = 13, CPU hoàn tất tác vụ 3 và bắt đầu xử lý tác vụ 2. Các tác vụ sẵn sàng = {0,1}.
- Tại thời điểm = 18, CPU hoàn tất tác vụ 2 và bắt đầu xử lý tác vụ 0. Các tác vụ sẵn sàng = {1}.
- Tại thời điểm = 28, CPU hoàn tất tác vụ 0 và bắt đầu xử lý tác vụ 1. Các tác vụ sẵn sàng = {}.
- Tại thời điểm = 40, CPU hoàn tất tác vụ 1 và trở nên rảnh.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong>​​​​​​​</p>

<ul>
	<li><code>1 &lt;= tasks.length &lt;= 10<sup>5</sup></code></li>
	<li><code>tasks[i] = [enqueueTime<sub>i</sub>, processingTime<sub>i</sub>]</code></li>
	<li><code>1 &lt;= enqueueTime<sub>i</sub>, processingTime<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> CPU rảnh sẽ chọn tác vụ đã đến có thời gian xử lý ngắn nhất, rồi đến tác vụ có chỉ số nhỏ nhất. Duyệt qua mọi tác vụ ở mỗi lần chọn có độ phức tạp $O(n^2)$, sẽ không đủ nhanh với $n\le 10^5$.
>
> Sắp xếp theo thời gian thêm vào và lưu các tác vụ đã đến trong min-heap gồm $(\textit{processingTime},\textit{index})$. Đưa thời gian hiện tại đến thời điểm tác vụ tiếp theo đến hoặc thời điểm hoàn tất tác vụ ở đỉnh heap, rồi thêm các tác vụ mới có thể xử lý. Heap trực tiếp hiện thực quy tắc lập lịch.

<!-- thinking:end -->

Đầu tiên, ta sắp xếp các tác vụ theo `enqueueTime` tăng dần. Tiếp theo, ta dùng priority queue (min heap) để duy trì các tác vụ hiện có thể thực thi. Các phần tử trong queue là `(processingTime, index)`, lần lượt biểu diễn thời gian thực thi và chỉ số của tác vụ. Ta cũng dùng biến $t$ để biểu diễn thời gian hiện tại, ban đầu đặt bằng $0$.

Tiếp theo, ta mô phỏng quá trình thực thi các tác vụ.

Nếu queue hiện tại rỗng, nghĩa là hiện không có tác vụ nào có thể thực thi. Ta cập nhật $t$ thành giá trị lớn hơn giữa `enqueueTime` của tác vụ tiếp theo và thời gian hiện tại $t$. Sau đó, ta thêm vào queue mọi tác vụ có `enqueueTime` nhỏ hơn hoặc bằng $t$.

Sau đó, ta lấy một tác vụ ra khỏi queue, thêm chỉ số của nó vào mảng đáp án, rồi cập nhật $t$ bằng tổng của thời gian hiện tại $t$ và thời gian thực thi của tác vụ hiện tại.

Ta lặp lại quy trình trên cho đến khi queue rỗng và tất cả tác vụ đã được thêm vào queue.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số lượng tác vụ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getOrder(self, tasks: List[List[int]]) -> List[int]:
        for i, task in enumerate(tasks):
            task.append(i)
        tasks.sort()
        ans = []
        q = []
        n = len(tasks)
        i = t = 0
        while q or i < n:
            if not q:
                t = max(t, tasks[i][0])
            while i < n and tasks[i][0] <= t:
                heappush(q, (tasks[i][1], tasks[i][2]))
                i += 1
            pt, j = heappop(q)
            ans.append(j)
            t += pt
        return ans
```

#### Java

```java
class Solution {
    public int[] getOrder(int[][] tasks) {
        int n = tasks.length;
        int[][] ts = new int[n][3];
        for (int i = 0; i < n; ++i) {
            ts[i] = new int[] {tasks[i][0], tasks[i][1], i};
        }
        Arrays.sort(ts, (a, b) -> a[0] - b[0]);
        int[] ans = new int[n];
        PriorityQueue<int[]> q
            = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int i = 0, t = 0, k = 0;
        while (!q.isEmpty() || i < n) {
            if (q.isEmpty()) {
                t = Math.max(t, ts[i][0]);
            }
            while (i < n && ts[i][0] <= t) {
                q.offer(new int[] {ts[i][1], ts[i][2]});
                ++i;
            }
            var p = q.poll();
            ans[k++] = p[1];
            t += p[0];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getOrder(vector<vector<int>>& tasks) {
        int n = 0;
        for (auto& task : tasks) task.push_back(n++);
        sort(tasks.begin(), tasks.end());
        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> q;
        int i = 0;
        long long t = 0;
        vector<int> ans;
        while (!q.empty() || i < n) {
            if (q.empty()) t = max(t, (long long) tasks[i][0]);
            while (i < n && tasks[i][0] <= t) {
                q.push({tasks[i][1], tasks[i][2]});
                ++i;
            }
            auto [pt, j] = q.top();
            q.pop();
            ans.push_back(j);
            t += pt;
        }
        return ans;
    }
};
```

#### Go

```go
func getOrder(tasks [][]int) (ans []int) {
	for i := range tasks {
		tasks[i] = append(tasks[i], i)
	}
	sort.Slice(tasks, func(i, j int) bool { return tasks[i][0] < tasks[j][0] })
	q := hp{}
	i, t, n := 0, 0, len(tasks)
	for len(q) > 0 || i < n {
		if len(q) == 0 {
			t = max(t, tasks[i][0])
		}
		for i < n && tasks[i][0] <= t {
			heap.Push(&q, pair{tasks[i][1], tasks[i][2]})
			i++
		}
		p := heap.Pop(&q).(pair)
		ans = append(ans, p.i)
		t += p.t
	}
	return
}

type pair struct{ t, i int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].t < h[j].t || (h[i].t == h[j].t && h[i].i < h[j].i) }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
