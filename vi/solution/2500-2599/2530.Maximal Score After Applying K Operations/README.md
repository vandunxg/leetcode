---
comments: true
difficulty: Medium
rating: 1386
source: Weekly Contest 327 Q2
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2530. Maximal Score After Applying K Operations](https://leetcode.com/problems/maximal-score-after-applying-k-operations)

[中文文档](/solution/2500-2599/2530.Maximal%20Score%20After%20Applying%20K%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong> và một số nguyên <code>k</code>. Ban đầu, <strong>điểm số</strong> của bạn là <code>0</code>.</p>

<p>Trong một <strong>thao tác</strong>:</p>

<ol>
	<li>chọn một chỉ số <code>i</code> sao cho <code>0 &lt;= i &lt; nums.length</code>,</li>
	<li>tăng <strong>điểm số</strong> thêm <code>nums[i]</code>, và</li>
	<li>thay <code>nums[i]</code> bằng <code>ceil(nums[i] / 3)</code>.</li>
</ol>

<p>Trả về <em><strong>điểm số</strong> lớn nhất có thể đạt được sau khi thực hiện <strong>chính xác</strong></em> <code>k</code> <em>thao tác</em>.</p>

<p>Hàm ceiling <code>ceil(val)</code> là số nguyên nhỏ nhất lớn hơn hoặc bằng <code>val</code>.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,10,10,10,10], k = 5
<strong>Đầu ra:</strong> 50
<strong>Giải thích:</strong> Thực hiện thao tác trên mỗi phần tử của mảng đúng một lần. Điểm số cuối cùng là 10 + 10 + 10 + 10 + 10 = 50.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,10,3,3,3], k = 3
<strong>Đầu ra:</strong> 17
<strong>Giải thích: </strong>Bạn có thể thực hiện các thao tác sau:
Thao tác 1: Chọn i = 1, khi đó nums trở thành [1,<strong><u>4</u></strong>,3,3,3]. Điểm số tăng thêm 10.
Thao tác 2: Chọn i = 1, khi đó nums trở thành [1,<strong><u>2</u></strong>,3,3,3]. Điểm số tăng thêm 4.
Thao tác 3: Chọn i = 2, khi đó nums trở thành [1,2,<u><strong>1</strong></u>,3,3]. Điểm số tăng thêm 3.
Điểm số cuối cùng là 10 + 4 + 3 = 17.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, k &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước cộng giá trị lớn nhất hiện tại $v$ vào điểm số rồi thay nó bằng $\lceil v/3\rceil$, lặp lại trong $k$ bước. Nếu mỗi lần đều duyệt để tìm giá trị lớn nhất thì độ phức tạp là bậc hai khi $k$ và $n$ đều đạt $10^5$.
>
> Max-heap luôn trả về phần tử lớn nhất hiện tại: lấy $v$ ra, cộng nó vào điểm số rồi đưa $\lceil v/3\rceil$ vào lại heap. Python lưu các giá trị âm. Vòng lặp thực hiện $k$ thao tác, mỗi thao tác có độ phức tạp logarit.

<!-- thinking:end -->

Để tối đa hóa tổng điểm, ta cần chọn phần tử có giá trị lớn nhất ở mỗi bước. Vì vậy, ta có thể dùng priority queue (max heap) để duy trì phần tử có giá trị lớn nhất.

Ở mỗi bước, ta lấy phần tử có giá trị lớn nhất $v$ ra khỏi priority queue, cộng $v$ vào đáp án, thay $v$ bằng $\lceil \frac{v}{3} \rceil$, rồi thêm nó trở lại priority queue. Sau khi lặp lại quy trình này $k$ lần, ta trả về đáp án.

Độ phức tạp thời gian là $O(n + k \times \log n)$, còn độ phức tạp không gian là $O(n)$ hoặc $O(1)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxKelements(self, nums: List[int], k: int) -> int:
        h = [-v for v in nums]
        heapify(h)
        ans = 0
        for _ in range(k):
            v = -heappop(h)
            ans += v
            heappush(h, -(ceil(v / 3)))
        return ans
```

#### Java

```java
class Solution {
    public long maxKelements(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        for (int v : nums) {
            pq.offer(v);
        }
        long ans = 0;
        while (k-- > 0) {
            int v = pq.poll();
            ans += v;
            pq.offer((v + 2) / 3);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxKelements(vector<int>& nums, int k) {
        priority_queue<int> pq(nums.begin(), nums.end());
        long long ans = 0;
        while (k--) {
            int v = pq.top();
            pq.pop();
            ans += v;
            pq.push((v + 2) / 3);
        }
        return ans;
    }
};
```

#### Go

```go
func maxKelements(nums []int, k int) (ans int64) {
	h := hp{nums}
	heap.Init(&h)
	for ; k > 0; k-- {
		ans += int64(h.IntSlice[0])
		h.IntSlice[0] = (h.IntSlice[0] + 2) / 3
		heap.Fix(&h, 0)
	}
	return
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (hp) Push(any)             {}
func (hp) Pop() (_ any)         { return }
```

#### TypeScript

```ts
function maxKelements(nums: number[], k: number): number {
    const pq = new MaxPriorityQueue<number>();
    nums.forEach(num => pq.enqueue(num));
    let ans = 0;
    while (k > 0) {
        const v = pq.dequeue();
        ans += v;
        pq.enqueue(Math.floor((v + 2) / 3));
        k--;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::BinaryHeap;

impl Solution {
    pub fn max_kelements(nums: Vec<i32>, k: i32) -> i64 {
        let mut pq = BinaryHeap::from(nums);
        let mut ans = 0;
        let mut k = k;
        while k > 0 {
            if let Some(v) = pq.pop() {
                ans += v as i64;
                pq.push((v + 2) / 3);
                k -= 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
