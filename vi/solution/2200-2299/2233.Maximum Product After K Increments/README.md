---
comments: true
difficulty: Medium
rating: 1685
source: Weekly Contest 288 Q3
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2233. Maximum Product After K Increments](https://leetcode.com/problems/maximum-product-after-k-increments)

[Tài liệu tiếng Trung](/solution/2200-2299/2233.Maximum%20Product%20After%20K%20Increments/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các số nguyên không âm <code>nums</code> và một số nguyên <code>k</code>. Trong một thao tác, bạn có thể chọn <strong>bất kỳ</strong> phần tử nào trong <code>nums</code> và <strong>tăng</strong> nó thêm <code>1</code>.</p>

<p>Trả về <em><strong>tích</strong> <strong>lớn nhất</strong> của </em><code>nums</code><em> sau <strong>không quá</strong> </em><code>k</code><em> thao tác. </em>Vì đáp án có thể rất lớn, hãy trả về nó sau khi lấy <b>modulo</b> <code>10<sup>9</sup> + 7</code>. Lưu ý rằng cần tối đa hóa tích trước khi lấy modulo.&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,4], k = 5
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Tăng số đầu tiên 5 lần.
Khi đó nums = [5, 4], với tích là 5 * 4 = 20.
Có thể chứng minh rằng 20 là tích lớn nhất có thể đạt được, nên ta trả về 20.
Lưu ý rằng có thể có những cách khác để tăng các phần tử của nums mà vẫn đạt tích lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,3,3,2], k = 2
<strong>Đầu ra:</strong> 216
<strong>Giải thích:</strong> Tăng số thứ hai 1 lần và tăng số thứ tư 1 lần.
Khi đó nums = [6, 4, 3, 3], với tích là 6 * 4 * 3 * 3 = 216.
Có thể chứng minh rằng 216 là tích lớn nhất có thể đạt được, nên ta trả về 216.
Lưu ý rằng có thể có những cách khác để tăng các phần tử của nums mà vẫn đạt tích lớn nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, k &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Priority Queue (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể tăng một phần tử thêm 1 trong mỗi thao tác và muốn tích đạt lớn nhất sau $k$ lần, trong đó $n$ và $k$ đều có thể lên tới $10^5$. Với một $x$ dương, mức tăng tương đối $\frac{x+1}{x}$ giảm dần khi $x$ lớn hơn, nên mỗi lần tăng cần tác động lên phần tử nhỏ nhất hiện tại.
>
> Min-heap lưu các phần tử của mảng; sau $k$ lần, ta thay phần tử đầu $x$ bằng $x+1$. Tích của các phần tử trong heap, sau khi lấy modulo, là đáp án. Các số 0 sẽ được tăng trước, nên tích không bị giữ ở 0.

<!-- thinking:end -->

Theo mô tả bài toán, để tối đa hóa tích, ta cần tăng các số nhỏ nhất nhiều nhất có thể. Vì vậy, ta có thể dùng min-heap để duy trì mảng $\textit{nums}$. Mỗi lần, ta lấy số nhỏ nhất từ min-heap, tăng nó thêm $1$, rồi đưa nó trở lại min-heap. Sau khi lặp lại quá trình này $k$ lần, ta nhân tất cả các số hiện có trong min-heap để nhận được đáp án.

Độ phức tạp thời gian là $O(k \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumProduct(self, nums: List[int], k: int) -> int:
        heapify(nums)
        for _ in range(k):
            heapreplace(nums, nums[0] + 1)
        mod = 10**9 + 7
        return reduce(lambda x, y: x * y % mod, nums)
```

#### Java

```java
class Solution {
    public int maximumProduct(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int x : nums) {
            pq.offer(x);
        }
        while (k-- > 0) {
            pq.offer(pq.poll() + 1);
        }
        final int mod = (int) 1e9 + 7;
        long ans = 1;
        for (int x : pq) {
            ans = (ans * x) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumProduct(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int x : nums) {
            pq.push(x);
        }
        while (k-- > 0) {
            int smallest = pq.top();
            pq.pop();
            pq.push(smallest + 1);
        }
        const int mod = 1e9 + 7;
        long long ans = 1;
        while (!pq.empty()) {
            ans = (ans * pq.top()) % mod;
            pq.pop();
        }
        return static_cast<int>(ans);
    }
};
```

#### Go

```go
func maximumProduct(nums []int, k int) int {
	h := hp{nums}
	for heap.Init(&h); k > 0; k-- {
		h.IntSlice[0]++
		heap.Fix(&h, 0)
	}
	ans := 1
	for _, x := range nums {
		ans = (ans * x) % (1e9 + 7)
	}
	return ans
}

type hp struct{ sort.IntSlice }

func (hp) Push(any)     {}
func (hp) Pop() (_ any) { return }
```

#### TypeScript

```ts
function maximumProduct(nums: number[], k: number): number {
    const pq = new MinPriorityQueue<number>();
    nums.forEach(x => pq.enqueue(x));
    while (k--) {
        const x = pq.dequeue();
        pq.enqueue(x + 1);
    }
    let ans = 1;
    const mod = 10 ** 9 + 7;
    while (!pq.isEmpty()) {
        ans = (ans * pq.dequeue()) % mod;
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var maximumProduct = function (nums, k) {
    const pq = new MinPriorityQueue();
    nums.forEach(x => pq.enqueue(x));
    while (k--) {
        const x = pq.dequeue();
        pq.enqueue(x + 1);
    }
    let ans = 1;
    const mod = 10 ** 9 + 7;
    while (!pq.isEmpty()) {
        ans = (ans * pq.dequeue()) % mod;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
