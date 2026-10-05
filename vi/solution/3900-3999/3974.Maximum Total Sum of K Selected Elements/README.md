---
comments: true
difficulty: Medium
rating: 1262
source: Weekly Contest 508 Q1
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3974. Maximum Total Sum of K Selected Elements](https://leetcode.com/problems/maximum-total-sum-of-k-selected-elements)

[中文文档](/solution/3900-3999/3974.Maximum%20Total%20Sum%20of%20K%20Selected%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>k</code> và <code>mul</code>.</p>

<p>Chọn <strong>chính xác</strong> <code>k</code> phần tử từ <code>nums</code>. Xử lý lần lượt các phần tử này theo bất kỳ thứ tự nào bạn chọn.</p>

<p>Với mỗi phần tử được chọn, <strong>độc lập</strong> chọn một trong các thao tác sau:</p>

<ul>
	<li><strong>Cộng</strong> giá trị của phần tử vào tổng, hoặc</li>
	<li><strong>Nhân</strong> phần tử với giá trị <strong>hiện tại</strong> của <code>mul</code> rồi <strong>cộng</strong> kết quả vào tổng.</li>
</ul>

<p>Sau khi xử lý mỗi phần tử được chọn, <code>mul</code> <strong>giảm</strong> đi 1, bất kể đã chọn phương án nào. Giá trị hiện tại của <code>mul</code> có thể trở thành 0 hoặc âm.</p>

<p>Hãy trả về một số nguyên biểu thị tổng <strong>lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,1,2,9], k = 3, mul = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">26</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách tối ưu là:</p>

<ul>
	<li>Một lựa chọn tối ưu là <code>nums[3] = 9</code>, <code>nums[0] = 6</code> và <code>nums[2] = 2</code>.</li>
	<li>Xử lý <code>nums[3] = 9</code> trước: chọn phép nhân nên phần tử đóng góp <code>9 * 2 = 18</code>. Khi đó, <code>mul</code> trở thành 1.</li>
	<li>Tiếp theo, xử lý <code>nums[0] = 6</code>: chọn phép nhân nên phần tử đóng góp <code>6 * 1 = 6</code>. Khi đó, <code>mul</code> trở thành 0.</li>
	<li>Cuối cùng, xử lý <code>nums[2] = 2</code>: chọn phép cộng nên phần tử đóng góp 2.</li>
	<li>Tổng là <code>18 + 6 + 2 = 26</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,7,5,2], k = 2, mul = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">43</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách tối ưu là:</p>

<ul>
	<li>Một lựa chọn tối ưu là <code>nums[1] = 7</code> và <code>nums[2] = 5</code>.</li>
	<li>Xử lý <code>nums[1] = 7</code> trước: chọn phép nhân nên phần tử đóng góp <code>7 * 4 = 28</code>. Khi đó, <code>mul</code> trở thành 3.</li>
	<li>Tiếp theo, xử lý <code>nums[2] = 5</code>: chọn phép nhân nên phần tử đóng góp <code>5 * 3 = 15</code>.</li>
	<li>Tổng là <code>28 + 15 = 43</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4], k = 1, mul = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách tối ưu là:</p>

<ul>
	<li>Một lựa chọn tối ưu là <code>nums[0] = 4</code>.</li>
	<li>Xử lý <code>nums[0] = 4</code>: chọn phép nhân nên phần tử đóng góp <code>4 * 1 = 4</code>.</li>
	<li>Tổng là 4.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
	<li><code>1 &lt;= mul &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần chọn, ta nhân với $\textit{mul}$ hiện tại rồi giảm nó đi một, nên các hệ số nhân giảm dần. Các giá trị lớn hơn trong mảng nên được ghép với các hệ số lớn hơn.
>
> Sắp xếp $\textit{nums}$ rồi lấy $k$ phần tử lớn nhất; phần tử lớn thứ $i$ được nhân với $\max(1,\textit{mul})$ trước khi $\textit{mul}$ giảm đi một. Độ phức tạp của việc sắp xếp là $O(n\log n)$.

<!-- thinking:end -->

Ta có thể sắp xếp mảng $\textit{nums}$ rồi chọn $k$ phần tử lớn nhất trong mảng đã sắp xếp. Với phần tử thứ $i$, ta có thể nhân nó với $\max(1, \textit{mul})$ rồi cộng vào tổng, sau đó giảm $\textit{mul}$ đi $1$. Cuối cùng, trả về tổng.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, nums: list[int], k: int, mul: int) -> int:
        nums.sort()
        n = len(nums)
        ans = 0
        for i in range(n - 1, n - 1 - k, -1):
            ans += nums[i] * max(1, mul)
            mul -= 1
        return ans
```

#### Java

```java
class Solution {
    public long maxSum(int[] nums, int k, int mul) {
        Arrays.sort(nums);
        int n = nums.length;
        long ans = 0;

        for (int i = n - 1; i >= n - k; i--) {
            int m = Math.max(1, mul);
            ans += (long) nums[i] * m;
            mul--;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxSum(vector<int>& nums, int k, int mul) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        long long ans = 0;

        for (int i = n - 1; i >= n - k; --i) {
            int m = max(1, mul);
            ans += 1LL * nums[i] * m;
            mul--;
        }

        return ans;
    }
};
```

#### Go

```go
func maxSum(nums []int, k int, mul int) int64 {
    sort.Ints(nums)
    n := len(nums)
    var ans int64 = 0

    for i := n - 1; i >= n-k; i-- {
        m := max(1, mul)
        ans += int64(nums[i]) * int64(m)
        mul--
    }

    return ans
}
```

#### TypeScript

```ts
function maxSum(nums: number[], k: number, mul: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = 0;

    for (let i = n - 1; i >= n - k; i--) {
        const m = Math.max(1, mul);
        ans += nums[i] * m;
        mul--;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
