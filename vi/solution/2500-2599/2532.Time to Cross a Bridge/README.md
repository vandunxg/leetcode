---
comments: true
difficulty: Hard
rating: 2588
source: Weekly Contest 327 Q4
tags:
    - Array
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2532. Time to Cross a Bridge](https://leetcode.com/problems/time-to-cross-a-bridge)

[中文文档](/solution/2500-2599/2532.Time%20to%20Cross%20a%20Bridge/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>k</code> công nhân muốn chuyển <code>n</code> thùng hàng từ kho bên phải (kho cũ) sang kho bên trái (kho mới). Cho hai số nguyên <code>n</code> và <code>k</code>, cùng một mảng số nguyên 2D <code>time</code> có kích thước <code>k x 4</code>, trong đó <code>time[i] = [right<sub>i</sub>, pick<sub>i</sub>, left<sub>i</sub>, put<sub>i</sub>]</code>.</p>

<p>Các kho bị ngăn cách bởi một con sông và được nối với nhau bằng một cây cầu. Ban đầu, tất cả <code>k</code> công nhân đều đang chờ ở phía bên trái cây cầu. Để chuyển các thùng hàng, công nhân thứ <code>i<sup>th</sup></code> có thể thực hiện các việc sau:</p>

<ul>
	<li>Đi qua cầu sang phía bên phải trong <code>right<sub>i</sub></code> phút.</li>
	<li>Lấy một thùng hàng từ kho bên phải trong <code>pick<sub>i</sub></code> phút.</li>
	<li>Đi qua cầu sang phía bên trái trong <code>left<sub>i</sub></code> phút.</li>
	<li>Đặt thùng hàng vào kho bên trái trong <code>put<sub>i</sub></code> phút.</li>
</ul>

<p>Công nhân thứ <code>i<sup>th</sup></code> <strong>kém hiệu quả hơn</strong> công nhân thứ j<code><sup>th</sup></code> nếu thỏa mãn một trong các điều kiện sau:</p>

<ul>
	<li><code>left<sub>i</sub> + right<sub>i</sub> &gt; left<sub>j</sub> + right<sub>j</sub></code></li>
	<li><code>left<sub>i</sub> + right<sub>i</sub> == left<sub>j</sub> + right<sub>j</sub></code> và <code>i &gt; j</code></li>
</ul>

<p>Các quy tắc sau chi phối việc công nhân đi qua cầu:</p>

<ul>
	<li>Mỗi lần chỉ có một công nhân được sử dụng cây cầu.</li>
	<li>Khi cầu đang không được sử dụng, ưu tiên công nhân <strong>kém hiệu quả nhất</strong> (đã lấy thùng hàng) ở phía bên phải đi qua cầu. Nếu không có công nhân nào như vậy, ưu tiên công nhân <strong>kém hiệu quả nhất</strong> ở phía bên trái đi qua cầu.</li>
	<li>Nếu đã điều động đủ công nhân từ phía bên trái để lấy tất cả các thùng hàng còn lại, thì <strong>không</strong> điều thêm công nhân nào từ phía bên trái.</li>
</ul>

<p>Trả về <strong>số phút đã trôi qua</strong> tại thời điểm thùng hàng cuối cùng đến <strong>phía bên trái cây cầu</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, k = 3, time = [[1,1,2,1],[1,1,3,1],[1,1,4,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<pre>
Từ phút 0 đến phút 1: công nhân 2 đi qua cầu sang bên phải.
Từ phút 1 đến phút 2: công nhân 2 lấy một thùng hàng từ kho bên phải.
Từ phút 2 đến phút 6: công nhân 2 đi qua cầu sang bên trái.
Từ phút 6 đến phút 7: công nhân 2 đặt một thùng hàng vào kho bên trái.
Toàn bộ quá trình kết thúc sau 7 phút. Ta trả về 6 vì đề bài yêu cầu thời điểm công nhân cuối cùng đến phía bên trái cây cầu.
</pre>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 2, time =</span> [[1,5,1,8],[10,10,10,10]]</p>

<p><strong>Đầu ra:</strong> 37</p>

<p><strong>Giải thích:</strong></p>

<pre>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2532.Time%20to%20Cross%20a%20Bridge/images/378539249-c6ce3c73-40e7-4670-a8b5-7ddb9abede11.png" style="width: 450px; height: 176px;" />
</pre>

<p>Thùng hàng cuối cùng đến phía bên trái ở giây thứ 37. Lưu ý rằng ta <strong>không</strong> đặt các thùng hàng cuối cùng xuống, vì việc đó sẽ tốn thêm thời gian và các thùng hàng đã ở bên trái cùng với công nhân.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, k &lt;= 10<sup>4</sup></code></li>
	<li><code>time.length == k</code></li>
	<li><code>time[i].length == 4</code></li>
	<li><code>1 &lt;= left<sub>i</sub>, pick<sub>i</sub>, right<sub>i</sub>, put<sub>i</sub> &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàng đợi ưu tiên (Max-Heap và Min-Heap) + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần chỉ một công nhân được đi qua cầu; phía bên phải được ưu tiên, và trên cùng một phía thì công nhân đi qua chậm hơn sẽ đi trước. Không thể mô phỏng từng giây khi thời gian có thể lên tới $10^9$.
>
> Ta sắp xếp công nhân theo tổng thời gian đi qua cầu để các chỉ số lớn hơn tương ứng với công nhân chậm hơn, rồi lưu các công nhân đang chờ trong các max-heap. Thời điểm hoàn thành việc lấy hoặc đặt thùng hàng được lưu trong các min-heap, sau đó công nhân được đưa trở lại heap chờ tương ứng. Nếu hiện tại không có ai có thể đi qua, ta nhảy đồng hồ đến thời điểm hoàn thành tiếp theo. Công nhân đang chờ ở bên phải sẽ đi qua trước; nếu không có, công nhân tiếp theo ở bên trái sẽ lấy một thùng hàng. Thời điểm thùng hàng cuối cùng quay về là đáp án.

<!-- thinking:end -->

Đầu tiên, ta sắp xếp công nhân theo hiệu quả giảm dần, để công nhân có chỉ số lớn nhất là công nhân kém hiệu quả nhất.

Tiếp theo, ta dùng bốn hàng đợi ưu tiên để mô phỏng trạng thái của các công nhân:

- `wait_in_left`: Max-heap, lưu chỉ số của các công nhân hiện đang chờ ở bờ trái;
- `wait_in_right`: Max-heap, lưu chỉ số của các công nhân hiện đang chờ ở bờ phải;
- `work_in_left`: Min-heap, lưu thời điểm các công nhân hiện đang làm việc ở bờ trái hoàn thành việc đặt thùng hàng và chỉ số của các công nhân đó;
- `work_in_right`: Min-heap, lưu thời điểm các công nhân hiện đang làm việc ở bờ phải hoàn thành việc lấy thùng hàng và chỉ số của các công nhân đó.

Ban đầu, tất cả công nhân đều ở bờ trái, nên `wait_in_left` lưu chỉ số của tất cả công nhân. Ta dùng biến `cur` để ghi nhận thời gian hiện tại.

Sau đó, ta mô phỏng toàn bộ quá trình. Trước tiên, ta kiểm tra xem có công nhân nào trong `work_in_left` đã hoàn thành việc đặt thùng hàng tại thời điểm hiện tại chưa. Nếu có, ta chuyển công nhân đó vào `wait_in_left` và xóa khỏi `work_in_left`. Tương tự, ta kiểm tra xem có công nhân nào trong `work_in_right` đã hoàn thành việc lấy thùng hàng chưa. Nếu có, ta chuyển công nhân đó vào `wait_in_right` và xóa khỏi `work_in_right`.

Tiếp theo, ta kiểm tra xem tại thời điểm hiện tại có công nhân nào đang chờ ở bờ trái hay không, ký hiệu là `left_to_go`. Đồng thời, ta kiểm tra xem có công nhân nào đang chờ ở bờ phải hay không, ký hiệu là `right_to_go`. Nếu không có công nhân nào đang chờ đi qua sông, ta cập nhật trực tiếp `cur` thành thời điểm tiếp theo mà một công nhân hoàn thành việc đặt thùng hàng rồi tiếp tục mô phỏng.

Nếu `right_to_go` là `true`, ta lấy một công nhân từ `wait_in_right`, cập nhật `cur` thành thời gian hiện tại cộng với thời gian công nhân đó đi từ bờ phải sang bờ trái. Nếu tại thời điểm này tất cả công nhân đã đi sang bờ phải, ta trả về trực tiếp `cur` làm đáp án; nếu không, ta chuyển công nhân đó vào `work_in_left`.

Nếu `left_to_go` là `true`, ta lấy một công nhân từ `wait_in_left`, cập nhật `cur` thành thời gian hiện tại cộng với thời gian công nhân đó đi từ bờ trái sang bờ phải, sau đó chuyển công nhân vào `work_in_right` và giảm số thùng hàng đi một đơn vị.

Lặp lại quá trình trên cho đến khi số thùng hàng bằng không. Khi đó, `cur` là đáp án.

Độ phức tạp thời gian là $O(n \times \log k)$, còn độ phức tạp không gian là $O(k)$. Trong đó, $n$ và $k$ lần lượt là số công nhân và số thùng hàng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findCrossingTime(self, n: int, k: int, time: List[List[int]]) -> int:
        time.sort(key=lambda x: x[0] + x[2])
        cur = 0
        wait_in_left, wait_in_right = [], []
        work_in_left, work_in_right = [], []
        for i in range(k):
            heappush(wait_in_left, -i)
        while 1:
            while work_in_left:
                t, i = work_in_left[0]
                if t > cur:
                    break
                heappop(work_in_left)
                heappush(wait_in_left, -i)
            while work_in_right:
                t, i = work_in_right[0]
                if t > cur:
                    break
                heappop(work_in_right)
                heappush(wait_in_right, -i)
            left_to_go = n > 0 and wait_in_left
            right_to_go = bool(wait_in_right)
            if not left_to_go and not right_to_go:
                nxt = inf
                if work_in_left:
                    nxt = min(nxt, work_in_left[0][0])
                if work_in_right:
                    nxt = min(nxt, work_in_right[0][0])
                cur = nxt
                continue
            if right_to_go:
                i = -heappop(wait_in_right)
                cur += time[i][2]
                if n == 0 and not wait_in_right and not work_in_right:
                    return cur
                heappush(work_in_left, (cur + time[i][3], i))
            else:
                i = -heappop(wait_in_left)
                cur += time[i][0]
                n -= 1
                heappush(work_in_right, (cur + time[i][1], i))
```

#### Java

```java
class Solution {
    public int findCrossingTime(int n, int k, int[][] time) {
        int[][] t = new int[k][5];
        for (int i = 0; i < k; ++i) {
            int[] x = time[i];
            t[i] = new int[] {x[0], x[1], x[2], x[3], i};
        }
        Arrays.sort(t, (a, b) -> {
            int x = a[0] + a[2], y = b[0] + b[2];
            return x == y ? a[4] - b[4] : x - y;
        });
        int cur = 0;
        PriorityQueue<Integer> waitInLeft = new PriorityQueue<>((a, b) -> b - a);
        PriorityQueue<Integer> waitInRight = new PriorityQueue<>((a, b) -> b - a);
        PriorityQueue<int[]> workInLeft = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        PriorityQueue<int[]> workInRight = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (int i = 0; i < k; ++i) {
            waitInLeft.offer(i);
        }
        while (true) {
            while (!workInLeft.isEmpty()) {
                int[] p = workInLeft.peek();
                if (p[0] > cur) {
                    break;
                }
                waitInLeft.offer(workInLeft.poll()[1]);
            }
            while (!workInRight.isEmpty()) {
                int[] p = workInRight.peek();
                if (p[0] > cur) {
                    break;
                }
                waitInRight.offer(workInRight.poll()[1]);
            }
            boolean leftToGo = n > 0 && !waitInLeft.isEmpty();
            boolean rightToGo = !waitInRight.isEmpty();
            if (!leftToGo && !rightToGo) {
                int nxt = 1 << 30;
                if (!workInLeft.isEmpty()) {
                    nxt = Math.min(nxt, workInLeft.peek()[0]);
                }
                if (!workInRight.isEmpty()) {
                    nxt = Math.min(nxt, workInRight.peek()[0]);
                }
                cur = nxt;
                continue;
            }
            if (rightToGo) {
                int i = waitInRight.poll();
                cur += t[i][2];
                if (n == 0 && waitInRight.isEmpty() && workInRight.isEmpty()) {
                    return cur;
                }
                workInLeft.offer(new int[] {cur + t[i][3], i});
            } else {
                int i = waitInLeft.poll();
                cur += t[i][0];
                --n;
                workInRight.offer(new int[] {cur + t[i][1], i});
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findCrossingTime(int n, int k, vector<vector<int>>& time) {
        using pii = pair<int, int>;
        for (int i = 0; i < k; ++i) {
            time[i].push_back(i);
        }
        sort(time.begin(), time.end(), [](auto& a, auto& b) {
            int x = a[0] + a[2], y = b[0] + b[2];
            return x == y ? a[4] < b[4] : x < y;
        });
        int cur = 0;
        priority_queue<int> waitInLeft, waitInRight;
        priority_queue<pii, vector<pii>, greater<pii>> workInLeft, workInRight;
        for (int i = 0; i < k; ++i) {
            waitInLeft.push(i);
        }
        while (true) {
            while (!workInLeft.empty()) {
                auto [t, i] = workInLeft.top();
                if (t > cur) {
                    break;
                }
                workInLeft.pop();
                waitInLeft.push(i);
            }
            while (!workInRight.empty()) {
                auto [t, i] = workInRight.top();
                if (t > cur) {
                    break;
                }
                workInRight.pop();
                waitInRight.push(i);
            }
            bool leftToGo = n > 0 && !waitInLeft.empty();
            bool rightToGo = !waitInRight.empty();
            if (!leftToGo && !rightToGo) {
                int nxt = 1 << 30;
                if (!workInLeft.empty()) {
                    nxt = min(nxt, workInLeft.top().first);
                }
                if (!workInRight.empty()) {
                    nxt = min(nxt, workInRight.top().first);
                }
                cur = nxt;
                continue;
            }
            if (rightToGo) {
                int i = waitInRight.top();
                waitInRight.pop();
                cur += time[i][2];
                if (n == 0 && waitInRight.empty() && workInRight.empty()) {
                    return cur;
                }
                workInLeft.push({cur + time[i][3], i});
            } else {
                int i = waitInLeft.top();
                waitInLeft.pop();
                cur += time[i][0];
                --n;
                workInRight.push({cur + time[i][1], i});
            }
        }
    }
};
```

#### Go

```go
func findCrossingTime(n int, k int, time [][]int) int {
	sort.SliceStable(time, func(i, j int) bool { return time[i][0]+time[i][2] < time[j][0]+time[j][2] })
	waitInLeft := hp{}
	waitInRight := hp{}
	workInLeft := hp2{}
	workInRight := hp2{}
	for i := range time {
		heap.Push(&waitInLeft, i)
	}
	cur := 0
	for {
		for len(workInLeft) > 0 {
			if workInLeft[0].t > cur {
				break
			}
			heap.Push(&waitInLeft, heap.Pop(&workInLeft).(pair).i)
		}
		for len(workInRight) > 0 {
			if workInRight[0].t > cur {
				break
			}
			heap.Push(&waitInRight, heap.Pop(&workInRight).(pair).i)
		}
		leftToGo := n > 0 && waitInLeft.Len() > 0
		rightToGo := waitInRight.Len() > 0
		if !leftToGo && !rightToGo {
			nxt := 1 << 30
			if len(workInLeft) > 0 {
				nxt = min(nxt, workInLeft[0].t)
			}
			if len(workInRight) > 0 {
				nxt = min(nxt, workInRight[0].t)
			}
			cur = nxt
			continue
		}
		if rightToGo {
			i := heap.Pop(&waitInRight).(int)
			cur += time[i][2]
			if n == 0 && waitInRight.Len() == 0 && len(workInRight) == 0 {
				return cur
			}
			heap.Push(&workInLeft, pair{cur + time[i][3], i})
		} else {
			i := heap.Pop(&waitInLeft).(int)
			cur += time[i][0]
			n--
			heap.Push(&workInRight, pair{cur + time[i][1], i})
		}
	}
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
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
func (h hp2) Less(i, j int) bool { return h[i].t < h[j].t }
func (h hp2) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp2) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp2) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
