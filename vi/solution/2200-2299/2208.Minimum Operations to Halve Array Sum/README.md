---
comments: true
difficulty: Medium
rating: 1550
source: Biweekly Contest 74 Q3
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2208. Minimum Operations to Halve Array Sum](https://leetcode.com/problems/minimum-operations-to-halve-array-sum)

[中文文档](/solution/2200-2299/2208.Minimum%20Operations%20to%20Halve%20Array%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các số nguyên dương <code>nums</code>. Trong một thao tác, bạn có thể chọn <strong>bất kỳ</strong> số nào trong <code>nums</code> và giảm nó xuống <strong>đúng bằng</strong> một nửa số đó. (Lưu ý rằng bạn có thể chọn số sau khi giảm này trong các thao tác tiếp theo.)</p>

<p>Trả về <em>số thao tác <strong>nhỏ nhất</strong> để giảm tổng của </em><code>nums</code><em> đi <strong>ít nhất</strong> một nửa.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,19,8,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Tổng ban đầu của nums bằng 5 + 19 + 8 + 1 = 33.
Sau đây là một cách để giảm tổng đi ít nhất một nửa:
Chọn số 19 và giảm nó xuống 9.5.
Chọn số 9.5 và giảm nó xuống 4.75.
Chọn số 8 và giảm nó xuống 4.
Mảng cuối cùng là [5, 4.75, 4, 1] với tổng bằng 5 + 4.75 + 4 + 1 = 14.75.
Tổng của nums đã giảm đi 33 - 14.75 = 18.25, lớn hơn hoặc bằng một nửa tổng ban đầu, 18.25 &gt;= 33/2 = 16.5.
Tổng cộng đã thực hiện 3 thao tác, nên ta trả về 3.
Có thể chứng minh rằng không thể giảm tổng đi ít nhất một nửa trong ít hơn 3 thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,8,20]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Tổng ban đầu của nums bằng 3 + 8 + 20 = 31.
Sau đây là một cách để giảm tổng đi ít nhất một nửa:
Chọn số 20 và giảm nó xuống 10.
Chọn số 10 và giảm nó xuống 5.
Chọn số 3 và giảm nó xuống 1.5.
Mảng cuối cùng là [1.5, 8, 5] với tổng bằng 1.5 + 8 + 5 = 14.5.
Tổng của nums đã giảm đi 31 - 14.5 = 16.5, lớn hơn hoặc bằng một nửa tổng ban đầu, 16.5 &gt;= 31/2 = 15.5.
Tổng cộng đã thực hiện 3 thao tác, nên ta trả về 3.
Có thể chứng minh rằng không thể giảm tổng đi ít nhất một nửa trong ít hơn 3 thao tác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác chia đôi một giá trị; ta muốn tổng giảm ít nhất một nửa với số bước ít nhất. Vì $n \le 10^5$, không thể tìm kiếm các chuỗi thao tác. Mức giảm bằng một nửa giá trị được chọn, nên ở mỗi bước ta luôn chọn giá trị lớn nhất hiện tại.
>
> Đưa các số vào max-heap và đặt $s = \mathrm{sum}(nums)/2$ là phần tổng còn cần giảm. Lặp lại: lấy $t$, trừ $t/2$ khỏi $s$, rồi đưa $t/2$ trở lại heap, cho đến khi $s \le 0$. Số lần lấy phần tử là đáp án.

<!-- thinking:end -->

Theo mô tả bài toán, mỗi thao tác sẽ chia đôi một số trong mảng. Để tối thiểu hóa số thao tác làm tổng mảng giảm ít nhất một nửa, ở mỗi thao tác ta nên chia đôi giá trị lớn nhất hiện tại trong mảng.

Vì vậy, trước tiên ta tính tổng $s$ mà mảng cần giảm, sau đó dùng priority queue (max heap) để duy trì tất cả các số trong mảng. Mỗi lần, ta lấy giá trị lớn nhất $t$ từ priority queue, chia đôi nó rồi đưa số đã chia đôi trở lại priority queue, đồng thời cập nhật $s$, cho đến khi $s \le 0$. Số thao tác tại thời điểm đó là đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def halveArray(self, nums: List[int]) -> int:
        s = sum(nums) / 2
        pq = []
        for x in nums:
            heappush(pq, -x)
        ans = 0
        while s > 0:
            t = -heappop(pq) / 2
            s -= t
            heappush(pq, -t)
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int halveArray(int[] nums) {
        PriorityQueue<Double> pq = new PriorityQueue<>(Collections.reverseOrder());
        double s = 0;
        for (int x : nums) {
            s += x;
            pq.offer((double) x);
        }
        s /= 2.0;
        int ans = 0;
        while (s > 0) {
            double t = pq.poll() / 2.0;
            s -= t;
            pq.offer(t);
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int halveArray(vector<int>& nums) {
        priority_queue<double> pq;
        double s = 0;
        for (int x : nums) {
            s += x;
            pq.push((double) x);
        }
        s /= 2.0;
        int ans = 0;
        while (s > 0) {
            double t = pq.top() / 2.0;
            pq.pop();
            s -= t;
            pq.push(t);
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func halveArray(nums []int) (ans int) {
	var s float64
	pq := &hp{}
	for _, x := range nums {
		s += float64(x)
		heap.Push(pq, float64(x))
	}
	s /= 2
	for s > 0 {
		t := heap.Pop(pq).(float64) / 2
		s -= t
		ans++
		heap.Push(pq, t)
	}
	return
}

type hp struct{ sort.Float64Slice }

func (h hp) Less(i, j int) bool { return h.Float64Slice[i] > h.Float64Slice[j] }
func (h *hp) Push(v any)        { h.Float64Slice = append(h.Float64Slice, v.(float64)) }
func (h *hp) Pop() any {
	a := h.Float64Slice
	v := a[len(a)-1]
	h.Float64Slice = a[:len(a)-1]
	return v
}
```

#### TypeScript

```ts
function halveArray(nums: number[]): number {
    let s: number = nums.reduce((a, b) => a + b) / 2;
    const pq = new MaxPriorityQueue<number>();
    for (const x of nums) {
        pq.enqueue(x);
    }
    let ans = 0;
    while (s > 0) {
        const t = pq.dequeue() / 2;
        s -= t;
        ++ans;
        pq.enqueue(t);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::BinaryHeap;

impl Solution {
    pub fn halve_array(nums: Vec<i32>) -> i32 {
        let mut pq: BinaryHeap<i64> = BinaryHeap::new();
        let mut s: i64 = 0;

        for x in nums {
            let v = (x as i64) << 20;
            s += v;
            pq.push(v);
        }

        s /= 2;
        let mut ans = 0;

        while s > 0 {
            let t = pq.pop().unwrap() / 2;
            s -= t;
            pq.push(t);
            ans += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
