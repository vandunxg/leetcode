---
comments: true
difficulty: Hard
rating: 2225
source: Biweekly Contest 71 Q4
tags:
    - Array
    - Dynamic Programming
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2163. Minimum Difference in Sums After Removal of Elements](https://leetcode.com/problems/minimum-difference-in-sums-after-removal-of-elements)

[中文文档](/solution/2100-2199/2163.Minimum%20Difference%20in%20Sums%20After%20Removal%20of%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số bắt đầu từ <strong>0</strong>, gồm <code>3 * n</code> phần tử.</p>

<p>Bạn được phép xóa một <strong>dãy con</strong> gồm <strong>chính xác</strong> <code>n</code> phần tử khỏi <code>nums</code>. <code>2 * n</code> phần tử còn lại sẽ được chia thành hai phần <strong>bằng nhau</strong>:</p>

<ul>
	<li><code>n</code> phần tử đầu tiên thuộc về phần thứ nhất, có tổng là <code>sum<sub>first</sub></code>.</li>
	<li><code>n</code> phần tử tiếp theo thuộc về phần thứ hai, có tổng là <code>sum<sub>second</sub></code>.</li>
</ul>

<p><strong>Hiệu tổng</strong> của hai phần được ký hiệu là <code>sum<sub>first</sub> - sum<sub>second</sub></code>.</p>

<ul>
	<li>Ví dụ, nếu <code>sum<sub>first</sub> = 3</code> và <code>sum<sub>second</sub> = 2</code>, hiệu của chúng là <code>1</code>.</li>
	<li>Tương tự, nếu <code>sum<sub>first</sub> = 2</code> và <code>sum<sub>second</sub> = 3</code>, hiệu của chúng là <code>-1</code>.</li>
</ul>

<p>Trả về <em><strong>hiệu nhỏ nhất</strong> có thể đạt được giữa tổng của hai phần sau khi xóa </em><code>n</code><em> phần tử</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,2]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Ở đây, nums có 3 phần tử, nên n = 1.
Vì vậy, ta phải xóa 1 phần tử khỏi nums rồi chia mảng thành hai phần bằng nhau.
- Nếu xóa nums[0] = 3, mảng sẽ là [1,2]. Hiệu tổng của hai phần là 1 - 2 = -1.
- Nếu xóa nums[1] = 1, mảng sẽ là [3,2]. Hiệu tổng của hai phần là 3 - 2 = 1.
- Nếu xóa nums[2] = 2, mảng sẽ là [3,1]. Hiệu tổng của hai phần là 3 - 1 = 2.
Hiệu nhỏ nhất giữa tổng của hai phần là min(-1,1,2) = -1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,9,5,8,1,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ở đây n = 2. Vì vậy, ta phải xóa 2 phần tử và chia các phần tử còn lại thành hai phần, mỗi phần gồm hai phần tử.
Nếu xóa nums[2] = 5 và nums[3] = 8, mảng thu được sẽ là [7,9,1,3]. Hiệu tổng là (7+9) - (1+3) = 12.
Để đạt được hiệu nhỏ nhất, ta nên xóa nums[1] = 9 và nums[4] = 1. Mảng thu được là [7,5,8,3]. Hiệu tổng của hai phần là (7+5) - (8+3) = 1.
Có thể chứng minh rằng không thể đạt được hiệu nhỏ hơn 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == 3 * n</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàng đợi ưu tiên (Max Heap và Min Heap) + Tổng tiền tố và hậu tố + Liệt kê các điểm chia

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi xóa $n$ phần tử, ta muốn hiệu giữa tổng của $n$ phần tử còn lại đầu tiên và $n$ phần tử cuối cùng là nhỏ nhất. Nói cách khác, ta cần tìm một điểm chia sao cho phần bên trái giữ lại $n$ phần tử nhỏ nhất và phần bên phải giữ lại $n$ phần tử lớn nhất. Duyệt vét cạn tập các phần tử bị xóa là không khả thi.
>
> Một max-heap duy trì tổng $\textit{pre}[i]$ của $n$ phần tử nhỏ nhất trong một tiền tố; một min-heap duy trì tổng $\textit{suf}[i]$ của $n$ phần tử lớn nhất trong một hậu tố. Tại điểm chia $i\in[n,2n]$, hiệu là $\textit{pre}[i]-\textit{suf}[i+1]$.
>
> Xây dựng cả hai mảng bằng heap, sau đó tìm giá trị nhỏ nhất trên các điểm chia.

<!-- thinking:end -->

Bài toán về cơ bản tương đương với việc tìm một điểm chia trong $nums$, chia mảng thành hai phần. Trong phần thứ nhất, chọn $n$ phần tử nhỏ nhất, còn trong phần thứ hai, chọn $n$ phần tử lớn nhất sao cho hiệu giữa tổng của hai phần là nhỏ nhất.

Ta có thể dùng một max heap để duy trì $n$ phần tử nhỏ nhất trong tiền tố và một min heap để duy trì $n$ phần tử lớn nhất trong hậu tố. Ta định nghĩa $pre[i]$ là tổng của $n$ phần tử nhỏ nhất trong $i$ phần tử đầu tiên của mảng $nums$, và $suf[i]$ là tổng của $n$ phần tử lớn nhất từ phần tử thứ $i$ đến phần tử cuối cùng của mảng. Trong quá trình duy trì max heap và min heap, ta cập nhật các giá trị của $pre[i]$ và $suf[i]$.

Cuối cùng, ta liệt kê các điểm chia trong khoảng $i \in [n, 2n]$, tính giá trị $pre[i] - suf[i + 1]$, rồi lấy giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDifference(self, nums: List[int]) -> int:
        m = len(nums)
        n = m // 3

        s = 0
        pre = [0] * (m + 1)
        q1 = []
        for i, x in enumerate(nums[: n * 2], 1):
            s += x
            heappush(q1, -x)
            if len(q1) > n:
                s -= -heappop(q1)
            pre[i] = s

        s = 0
        suf = [0] * (m + 1)
        q2 = []
        for i in range(m, n, -1):
            x = nums[i - 1]
            s += x
            heappush(q2, x)
            if len(q2) > n:
                s -= heappop(q2)
            suf[i] = s

        return min(pre[i] - suf[i + 1] for i in range(n, n * 2 + 1))
```

#### Java

```java
class Solution {
    public long minimumDifference(int[] nums) {
        int m = nums.length;
        int n = m / 3;
        long s = 0;
        long[] pre = new long[m + 1];
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        for (int i = 1; i <= n * 2; ++i) {
            int x = nums[i - 1];
            s += x;
            pq.offer(x);
            if (pq.size() > n) {
                s -= pq.poll();
            }
            pre[i] = s;
        }
        s = 0;
        long[] suf = new long[m + 1];
        pq = new PriorityQueue<>();
        for (int i = m; i > n; --i) {
            int x = nums[i - 1];
            s += x;
            pq.offer(x);
            if (pq.size() > n) {
                s -= pq.poll();
            }
            suf[i] = s;
        }
        long ans = 1L << 60;
        for (int i = n; i <= n * 2; ++i) {
            ans = Math.min(ans, pre[i] - suf[i + 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumDifference(vector<int>& nums) {
        int m = nums.size();
        int n = m / 3;

        using ll = long long;
        ll s = 0;
        ll pre[m + 1];
        priority_queue<int> q1;
        for (int i = 1; i <= n * 2; ++i) {
            int x = nums[i - 1];
            s += x;
            q1.push(x);
            if (q1.size() > n) {
                s -= q1.top();
                q1.pop();
            }
            pre[i] = s;
        }
        s = 0;
        ll suf[m + 1];
        priority_queue<int, vector<int>, greater<int>> q2;
        for (int i = m; i > n; --i) {
            int x = nums[i - 1];
            s += x;
            q2.push(x);
            if (q2.size() > n) {
                s -= q2.top();
                q2.pop();
            }
            suf[i] = s;
        }
        ll ans = 1e18;
        for (int i = n; i <= n * 2; ++i) {
            ans = min(ans, pre[i] - suf[i + 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumDifference(nums []int) int64 {
	m := len(nums)
	n := m / 3
	s := 0
	pre := make([]int, m+1)
	q1 := hp{}
	for i := 1; i <= n*2; i++ {
		x := nums[i-1]
		s += x
		heap.Push(&q1, -x)
		if q1.Len() > n {
			s -= -heap.Pop(&q1).(int)
		}
		pre[i] = s
	}
	s = 0
	suf := make([]int, m+1)
	q2 := hp{}
	for i := m; i > n; i-- {
		x := nums[i-1]
		s += x
		heap.Push(&q2, x)
		if q2.Len() > n {
			s -= heap.Pop(&q2).(int)
		}
		suf[i] = s
	}
	ans := int64(1e18)
	for i := n; i <= n*2; i++ {
		ans = min(ans, int64(pre[i]-suf[i+1]))
	}
	return ans
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
```

#### TypeScript

```ts
function minimumDifference(nums: number[]): number {
    const m = nums.length;
    const n = Math.floor(m / 3);
    let s = 0;
    const pre: number[] = Array(m + 1);
    const q1 = new MaxPriorityQueue<number>();
    for (let i = 1; i <= n * 2; ++i) {
        const x = nums[i - 1];
        s += x;
        q1.enqueue(x);
        if (q1.size() > n) {
            s -= q1.dequeue();
        }
        pre[i] = s;
    }
    s = 0;
    const suf: number[] = Array(m + 1);
    const q2 = new MinPriorityQueue<number>();
    for (let i = m; i > n; --i) {
        const x = nums[i - 1];
        s += x;
        q2.enqueue(x);
        if (q2.size() > n) {
            s -= q2.dequeue();
        }
        suf[i] = s;
    }
    let ans = Number.MAX_SAFE_INTEGER;
    for (let i = n; i <= n * 2; ++i) {
        ans = Math.min(ans, pre[i] - suf[i + 1]);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::BinaryHeap;
use std::cmp::Reverse;

impl Solution {
    pub fn minimum_difference(nums: Vec<i32>) -> i64 {
        let m = nums.len();
        let n = m / 3;
        let mut s = 0i64;
        let mut pre = vec![0i64; m + 1];
        let mut pq = BinaryHeap::new(); // max-heap

        for i in 1..=2 * n {
            let x = nums[i - 1] as i64;
            s += x;
            pq.push(x);
            if pq.len() > n {
                if let Some(top) = pq.pop() {
                    s -= top;
                }
            }
            pre[i] = s;
        }

        s = 0;
        let mut suf = vec![0i64; m + 1];
        let mut pq = BinaryHeap::new();

        for i in (n + 1..=m).rev() {
            let x = nums[i - 1] as i64;
            s += x;
            pq.push(Reverse(x));
            if pq.len() > n {
                if let Some(Reverse(top)) = pq.pop() {
                    s -= top;
                }
            }
            suf[i] = s;
        }

        let mut ans = i64::MAX;
        for i in n..=2 * n {
            ans = ans.min(pre[i] - suf[i + 1]);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
