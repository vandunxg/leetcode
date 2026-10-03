---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [2495. Number of Subarrays Having Even Product 🔒](https://leetcode.com/problems/number-of-subarrays-having-even-product)

[中文文档](/solution/2400-2499/2495.Number%20of%20Subarrays%20Having%20Even%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>, hãy trả về <em>số lượng <span data-keyword="subarray-nonempty">mảng con</span> của </em><code>nums</code><em> có tích là số chẵn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,6,7,13]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có 6 mảng con có tích là số chẵn:
- nums[0..1] = 9 * 6 = 54.
- nums[0..2] = 9 * 6 * 7 = 378.
- nums[0..3] = 9 * 6 * 7 * 13 = 4914.
- nums[1..1] = 6.
- nums[1..2] = 6 * 7 = 42.
- nums[1..3] = 6 * 7 * 13 = 546.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,3,5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng con nào có tích là số chẵn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Tích của một mảng con là số chẵn khi và chỉ khi nó chứa một số chẵn. Với đầu phải $i$, đầu trái có thể là bất kỳ chỉ số nào không vượt quá số chẵn gần nhất, tức có $last+1$ lựa chọn ($0$ nếu không có). Ta chỉ cần duy trì $last$ trong một lần duyệt.

<!-- thinking:end -->

Ta biết rằng tích của một mảng con là số chẵn khi và chỉ khi trong mảng con có ít nhất một số chẵn.

Vì vậy, ta có thể duyệt qua mảng, lưu chỉ số `last` của số chẵn gần nhất, khi đó số lượng mảng con kết thúc tại phần tử hiện tại và có tích là số chẵn là `last + 1`. Ta cộng giá trị này vào kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng `nums`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def evenProduct(self, nums: List[int]) -> int:
        ans, last = 0, -1
        for i, v in enumerate(nums):
            if v % 2 == 0:
                last = i
            ans += last + 1
        return ans
```

#### Java

```java
class Solution {
    public long evenProduct(int[] nums) {
        long ans = 0;
        int last = -1;
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] % 2 == 0) {
                last = i;
            }
            ans += last + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long evenProduct(vector<int>& nums) {
        long long ans = 0;
        int last = -1;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] % 2 == 0) {
                last = i;
            }
            ans += last + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func evenProduct(nums []int) int64 {
	ans, last := 0, -1
	for i, v := range nums {
		if v%2 == 0 {
			last = i
		}
		ans += last + 1
	}
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
