---
comments: true
difficulty: Medium
rating: 1823
source: Biweekly Contest 172 Q3
tags:
    - Greedy
    - Array
    - String
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3781. Maximum Score After Binary Swaps](https://leetcode.com/problems/maximum-score-after-binary-swaps)

[中文文档](/solution/3700-3799/3781.Maximum%20Score%20After%20Binary%20Swaps/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một chuỗi nhị phân <code>s</code> có cùng độ dài.</p>

<p>Ban đầu, điểm số của bạn là 0. Mỗi chỉ số <code>i</code> sao cho <code>s[i] = &#39;1&#39;</code> đóng góp <code>nums[i]</code> vào điểm số.</p>

<p>Bạn có thể thực hiện <strong>bất kỳ số lượng thao tác</strong> nào, kể cả không thực hiện thao tác nào. Trong một thao tác, bạn có thể chọn một chỉ số <code>i</code> sao cho <code>0 &lt;= i &lt; n - 1</code>, <code>s[i] = &#39;0&#39;</code> và <code>s[i + 1] = &#39;1&#39;</code>, rồi hoán đổi hai ký tự này.</p>

<p>Trả về một số nguyên biểu thị <strong>điểm số lớn nhất có thể đạt được</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,5,2,3], s = &quot;01010&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thực hiện các thao tác hoán đổi sau:</p>

<ul>
	<li>Hoán đổi tại chỉ số <code>i = 0</code>: <code>&quot;01010&quot;</code> đổi thành <code>&quot;10010&quot;</code></li>
	<li>Hoán đổi tại chỉ số <code>i = 2</code>: <code>&quot;10010&quot;</code> đổi thành <code>&quot;10100&quot;</code></li>
</ul>

<p>Các vị trí 0 và 2 chứa <code>&#39;1&#39;</code>, đóng góp <code>nums[0] + nums[2] = 2 + 5 = 7</code>. Đây là điểm số lớn nhất có thể đạt được.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,7,2,9], s = &quot;0000&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có ký tự <code>&#39;1&#39;</code> nào trong <code>s</code>, nên không thể thực hiện thao tác hoán đổi. Điểm số vẫn là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length == s.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Max-Heap

<!-- thinking:start -->

> **Tư duy**
>
> Một `1` chỉ có thể hoán đổi với một `0` ở bên trái, vì vậy mỗi `1` có thể lấy giá trị chưa được dùng lớn nhất trong các vị trí đã duyệt. Khi duyệt từ trái sang phải, ta đưa $nums[i]$ vào max-heap và khi gặp một `1`, lấy phần tử lớn nhất hiện tại ra để cộng vào điểm số.

<!-- thinking:end -->

Theo đề bài, mỗi `'1'` có thể được hoán đổi sang trái bao nhiêu lần tùy ý, nên mỗi `'1'` có thể chọn số lớn nhất chưa được lấy ở bên trái nó. Ta có thể quản lý các ứng viên này bằng một max-heap.

Duyệt chuỗi $s$: với mỗi vị trí $i$, đưa số tương ứng $\textit{nums}[i]$ vào max-heap; nếu $s[i] = '1'$, lấy phần tử lớn nhất ra khỏi heap và cộng vào đáp án.

Sau khi duyệt xong, tổng tích lũy là điểm số lớn nhất.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, nums: List[int], s: str) -> int:
        ans = 0
        pq = []
        for x, c in zip(nums, s):
            heappush(pq, -x)
            if c == "1":
                ans -= heappop(pq)
        return ans
```

#### Java

```java
class Solution {
    public long maximumScore(int[] nums, String s) {
        long ans = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
        for (int i = 0; i < nums.length; i++) {
            int x = nums[i];
            char c = s.charAt(i);
            pq.offer(x);
            if (c == '1') {
                ans += pq.poll();
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
    long long maximumScore(vector<int>& nums, string s) {
        long long ans = 0;
        priority_queue<int> pq;
        for (int i = 0; i < nums.size(); i++) {
            int x = nums[i];
            char c = s[i];
            pq.push(x);
            if (c == '1') {
                ans += pq.top();
                pq.pop();
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumScore(nums []int, s string) int64 {
	var ans int64
	pq := &hp{}
	heap.Init(pq)
	for i, x := range nums {
		pq.push(x)
		if s[i] == '1' {
			ans += int64(pq.pop())
		}
	}
	return ans
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
func (h *hp) push(v int) { heap.Push(h, v) }
func (h *hp) pop() int   { return heap.Pop(h).(int) }
```

#### TypeScript

```ts
function maximumScore(nums: number[], s: string): number {
    let ans = 0;
    const pq = new MaxPriorityQueue<number>();

    for (let i = 0; i < nums.length; i++) {
        const x = nums[i];
        const c = s[i];
        pq.enqueue(x);
        if (c === '1') {
            ans += pq.dequeue()!;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
