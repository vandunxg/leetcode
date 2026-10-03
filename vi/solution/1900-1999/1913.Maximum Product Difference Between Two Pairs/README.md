---
comments: true
difficulty: Easy
rating: 1144
source: Weekly Contest 247 Q1
tags:
    - Array
    - Sorting
    - Quick Sort
---

<!-- problem:start -->

# [1913. Maximum Product Difference Between Two Pairs](https://leetcode.com/problems/maximum-product-difference-between-two-pairs)

[中文文档](/solution/1900-1999/1913.Maximum%20Product%20Difference%20Between%20Two%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Hiệu tích</strong> giữa hai cặp <code>(a, b)</code> và <code>(c, d)</code> được định nghĩa là <code>(a * b) - (c * d)</code>.</p>

<ul>
	<li>Ví dụ, hiệu tích giữa <code>(5, 6)</code> và <code>(2, 7)</code> là <code>(5 * 6) - (2 * 7) = 16</code>.</li>
</ul>

<p>Cho một mảng số nguyên <code>nums</code>, hãy chọn bốn chỉ số <strong>phân biệt</strong> <code>w</code>, <code>x</code>, <code>y</code> và <code>z</code> sao cho <strong>hiệu tích</strong> giữa hai cặp <code>(nums[w], nums[x])</code> và <code>(nums[y], nums[z])</code> đạt giá trị <strong>lớn nhất</strong>.</p>

<p>Trả về <em><strong>hiệu tích lớn nhất</strong> đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,6,2,7,4]
<strong>Đầu ra:</strong> 34
<strong>Giải thích:</strong> Ta có thể chọn các chỉ số 1 và 3 cho cặp đầu tiên (6, 7), và các chỉ số 2 và 4 cho cặp thứ hai (2, 4).
Hiệu tích là (6 * 7) - (2 * 4) = 34.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,5,9,7,4,8]
<strong>Đầu ra:</strong> 64
<strong>Giải thích:</strong> Ta có thể chọn các chỉ số 3 và 6 cho cặp đầu tiên (9, 8), và các chỉ số 1 và 5 cho cặp thứ hai (2, 4).
Hiệu tích là (9 * 8) - (2 * 4) = 64.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>4 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê bốn chỉ số có độ phức tạp $O(n^4)$. Để tối đa hóa $ab-cd$, ta cần lấy tích lớn nhất trừ đi tích nhỏ nhất.
>
> Sau khi sắp xếp, đó là tích của hai phần tử lớn nhất trừ đi tích của hai phần tử nhỏ nhất; bốn chỉ số này là phân biệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProductDifference(self, nums: List[int]) -> int:
        nums.sort()
        return nums[-1] * nums[-2] - nums[0] * nums[1]
```

#### Java

```java
class Solution {
    public int maxProductDifference(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        return nums[n - 1] * nums[n - 2] - nums[0] * nums[1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProductDifference(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        return nums[n - 1] * nums[n - 2] - nums[0] * nums[1];
    }
};
```

#### Go

```go
func maxProductDifference(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	return nums[n-1]*nums[n-2] - nums[0]*nums[1]
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProductDifference = function (nums) {
    nums.sort((a, b) => a - b);
    let n = nums.length;
    let ans = nums[n - 1] * nums[n - 2] - nums[0] * nums[1];
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
