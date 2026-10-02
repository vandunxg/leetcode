---
comments: true
difficulty: Easy
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [908. Smallest Range I](https://leetcode.com/problems/smallest-range-i)

[中文文档](/solution/0900-0999/0908.Smallest%20Range%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể chọn chỉ số bất kỳ <code>i</code> thỏa mãn <code>0 &lt;= i &lt; nums.length</code> và đổi <code>nums[i]</code> thành <code>nums[i] + x</code>, trong đó <code>x</code> là số nguyên thuộc đoạn <code>[-k, k]</code>. Với mỗi chỉ số <code>i</code>, bạn chỉ được thực hiện thao tác này <strong>nhiều nhất một lần</strong>.</p>

<p><strong>Điểm số</strong> của <code>nums</code> là hiệu giữa phần tử lớn nhất và nhỏ nhất trong mảng.</p>

<p>Hãy trả về <em><strong>điểm số</strong> nhỏ nhất của </em><code>nums</code><em> sau khi áp dụng thao tác trên nhiều nhất một lần cho mỗi chỉ số</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1], k = 0
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Điểm số là max(nums) - min(nums) = 1 - 1 = 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [0,10], k = 2
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Đổi nums thành [2, 8]. Điểm số là max(nums) - min(nums) = 8 - 2 = 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,3,6], k = 3
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Đổi nums thành [4, 4, 4]. Điểm số là max(nums) - min(nums) = 4 - 4 = 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giá trị có thể dịch chuyển nhiều nhất $k$, còn điểm số cuối cùng là khoảng cách giữa giá trị lớn nhất và nhỏ nhất mới. Dịch chuyển tất cả giá trị cùng một lượng không làm thay đổi khoảng cách. Ta có thể giảm giá trị lớn nhất và tăng giá trị nhỏ nhất để thu hẹp khoảng cách tối đa, khi đó kết quả là $\max(0,\max(nums)-\min(nums)-2k)$.

<!-- thinking:end -->

Theo đề bài, ta có thể trừ $k$ khỏi giá trị lớn nhất và cộng $k$ vào giá trị nhỏ nhất trong mảng để giảm hiệu giữa chúng.

Vì vậy, đáp án cuối cùng là giá trị lớn hơn giữa $\max(\textit{nums}) - \min(\textit{nums}) - 2 \times k$ và $0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestRangeI(self, nums: List[int], k: int) -> int:
        mx, mi = max(nums), min(nums)
        return max(0, mx - mi - k * 2)
```

#### Java

```java
class Solution {
    public int smallestRangeI(int[] nums, int k) {
        int mx = 0;
        int mi = 10000;
        for (int v : nums) {
            mx = Math.max(mx, v);
            mi = Math.min(mi, v);
        }
        return Math.max(0, mx - mi - k * 2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestRangeI(vector<int>& nums, int k) {
        auto [mi, mx] = minmax_element(nums.begin(), nums.end());
        return max(0, *mx - *mi - k * 2);
    }
};
```

#### Go

```go
func smallestRangeI(nums []int, k int) int {
	mi, mx := slices.Min(nums), slices.Max(nums)
	return max(0, mx-mi-k*2)
}
```

#### TypeScript

```ts
function smallestRangeI(nums: number[], k: number): number {
    const mx = Math.max(...nums);
    const mi = Math.min(...nums);
    return Math.max(mx - mi - k * 2, 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_range_i(nums: Vec<i32>, k: i32) -> i32 {
        let max = nums.iter().max().unwrap();
        let min = nums.iter().min().unwrap();
        (0).max(max - min - k * 2)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
