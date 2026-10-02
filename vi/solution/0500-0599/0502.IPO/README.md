---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [502. IPO](https://leetcode.com/problems/ipo)

[中文文档](/solution/0500-0599/0502.IPO/README.md)

## Mô tả

<!-- description:start -->

<p>Giả sử LeetCode sắp tiến hành <strong>IPO</strong>. Để bán cổ phiếu với giá tốt cho các quỹ đầu tư mạo hiểm, LeetCode muốn thực hiện một số dự án để tăng vốn trước <strong>IPO</strong>. Vì nguồn lực có hạn, LeetCode chỉ có thể hoàn thành tối đa <code>k</code> dự án khác nhau trước thời điểm đó. Hãy giúp LeetCode tìm cách tối ưu để tối đa hóa tổng vốn sau khi hoàn thành tối đa <code>k</code> dự án khác nhau.</p>

<p>Bạn được cho <code>n</code> dự án. Dự án thứ <code>i</code> có lợi nhuận ròng <code>profits[i]</code> và cần số vốn tối thiểu <code>capital[i]</code> để bắt đầu.</p>

<p>Ban đầu, bạn có số vốn <code>w</code>. Khi hoàn thành một dự án, bạn nhận được lợi nhuận ròng của dự án đó và cộng khoản lợi nhuận này vào tổng vốn.</p>

<p>Hãy chọn tối đa <code>k</code> dự án khác nhau trong số các dự án đã cho để <strong>tối đa hóa số vốn cuối cùng</strong>, rồi trả về <em>số vốn cuối cùng lớn nhất</em>.</p>

<p>Đảm bảo kết quả nằm trong phạm vi số nguyên có dấu 32-bit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Vì số vốn ban đầu bằng 0, bạn chỉ có thể bắt đầu dự án ở chỉ số 0.
Sau khi hoàn thành dự án này, bạn nhận được lợi nhuận 1 và số vốn tăng lên 1.
Với số vốn 1, bạn có thể bắt đầu dự án ở chỉ số 1 hoặc dự án ở chỉ số 2.
Vì bạn được chọn tối đa 2 dự án, để đạt số vốn lớn nhất, hãy hoàn thành dự án ở chỉ số 2.
Vì vậy, số vốn cuối cùng lớn nhất cần trả về là 0 + 1 + 3 = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 3, w = 0, profits = [1,2,3], capital = [0,1,2]
<strong>Đầu ra:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= w &lt;= 10<sup>9</sup></code></li>
	<li><code>n == profits.length</code></li>
	<li><code>n == capital.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= profits[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= capital[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ở mỗi lượt trong $k$ lượt chọn, ta nên chọn dự án có lợi nhuận cao nhất mà số vốn hiện tại cho phép bắt đầu. Duyệt cả $n$ dự án ở mỗi lượt sẽ tốn $O(kn)$, quá nhiều khi $n,k \le 10^5$.
>
> Số vốn không bao giờ giảm, vì vậy ta lưu các dự án chưa đủ vốn trong min-heap theo yêu cầu vốn, rồi chuyển những dự án đã đủ điều kiện vào max-heap theo lợi nhuận. Mỗi lượt lấy dự án có lợi nhuận cao nhất ra và cập nhật vốn. Hai heap lần lượt cho biết dự án tiếp theo có thể mở khóa và lựa chọn tốt nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaximizedCapital(
        self, k: int, w: int, profits: List[int], capital: List[int]
    ) -> int:
        h1 = [(c, p) for c, p in zip(capital, profits)]
        heapify(h1)
        h2 = []
        while k:
            while h1 and h1[0][0] <= w:
                heappush(h2, -heappop(h1)[1])
            if not h2:
                break
            w -= heappop(h2)
            k -= 1
        return w
```

#### Java

```java
class Solution {
    public int findMaximizedCapital(int k, int w, int[] profits, int[] capital) {
        int n = capital.length;
        PriorityQueue<int[]> q1 = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (int i = 0; i < n; ++i) {
            q1.offer(new int[] {capital[i], profits[i]});
        }
        PriorityQueue<Integer> q2 = new PriorityQueue<>((a, b) -> b - a);
        while (k-- > 0) {
            while (!q1.isEmpty() && q1.peek()[0] <= w) {
                q2.offer(q1.poll()[1]);
            }
            if (q2.isEmpty()) {
                break;
            }
            w += q2.poll();
        }
        return w;
    }
}
```

#### C++

```cpp
using pii = pair<int, int>;

class Solution {
public:
    int findMaximizedCapital(int k, int w, vector<int>& profits, vector<int>& capital) {
        priority_queue<pii, vector<pii>, greater<pii>> q1;
        int n = profits.size();
        for (int i = 0; i < n; ++i) {
            q1.push({capital[i], profits[i]});
        }
        priority_queue<int> q2;
        while (k--) {
            while (!q1.empty() && q1.top().first <= w) {
                q2.push(q1.top().second);
                q1.pop();
            }
            if (q2.empty()) {
                break;
            }
            w += q2.top();
            q2.pop();
        }
        return w;
    }
};
```

#### Go

```go
func findMaximizedCapital(k int, w int, profits []int, capital []int) int {
	q1 := hp2{}
	for i, c := range capital {
		heap.Push(&q1, pair{c, profits[i]})
	}
	q2 := hp{}
	for k > 0 {
		for len(q1) > 0 && q1[0].c <= w {
			heap.Push(&q2, heap.Pop(&q1).(pair).p)
		}
		if q2.Len() == 0 {
			break
		}
		w += heap.Pop(&q2).(int)
		k--
	}
	return w
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

type pair struct{ c, p int }
type hp2 []pair

func (h hp2) Len() int           { return len(h) }
func (h hp2) Less(i, j int) bool { return h[i].c < h[j].c }
func (h hp2) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp2) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp2) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
