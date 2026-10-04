---
comments: true
difficulty: Easy
rating: 1151
source: Weekly Contest 354 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [2778. Sum of Squares of Special Elements](https://leetcode.com/problems/sum-of-squares-of-special-elements)

[中文文档](/solution/2700-2799/2778.Sum%20of%20Squares%20of%20Special%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, với chỉ số <strong>bắt đầu từ 1</strong>.</p>

<p>Một phần tử <code>nums[i]</code> của <code>nums</code> được gọi là <strong>đặc biệt</strong> nếu <code>i</code> là ước của <code>n</code>, tức là <code>n % i == 0</code>.</p>

<p>Trả về <em><strong>tổng bình phương</strong> của tất cả các phần tử <strong>đặc biệt</strong> trong </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong> Trong nums có đúng 3 phần tử đặc biệt: nums[1] vì 1 là ước của 4, nums[2] vì 2 là ước của 4 và nums[4] vì 4 là ước của 4.
Do đó, tổng bình phương của tất cả các phần tử đặc biệt trong nums là nums[1] * nums[1] + nums[2] * nums[2] + nums[4] * nums[4] = 1 * 1 + 2 * 2 + 4 * 4 = 21.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,7,1,19,18,3]
<strong>Đầu ra:</strong> 63
<strong>Giải thích:</strong> Trong nums có đúng 4 phần tử đặc biệt: nums[1] vì 1 là ước của 6, nums[2] vì 2 là ước của 6, nums[3] vì 3 là ước của 6 và nums[6] vì 6 là ước của 6.
Do đó, tổng bình phương của tất cả các phần tử đặc biệt trong nums là nums[1] * nums[1] + nums[2] * nums[2] + nums[3] * nums[3] + nums[6] * nums[6] = 2 * 2 + 7 * 7 + 1 * 1 + 3 * 3 = 63.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length == n &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một phần tử đặc biệt là phần tử có chỉ số $1$-based là ước của $n$; ta cần tính tổng bình phương của chúng. Không cần thu thập các chỉ số trước.
>
> Duyệt $i=1..n$ và cộng $nums[i-1]^2$ khi $n\bmod i=0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfSquares(self, nums: List[int]) -> int:
        n = len(nums)
        return sum(x * x for i, x in enumerate(nums, 1) if n % i == 0)
```

#### Java

```java
class Solution {
    public int sumOfSquares(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            if (n % i == 0) {
                ans += nums[i - 1] * nums[i - 1];
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
    int sumOfSquares(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            if (n % i == 0) {
                ans += nums[i - 1] * nums[i - 1];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfSquares(nums []int) (ans int) {
	n := len(nums)
	for i, x := range nums {
		if n%(i+1) == 0 {
			ans += x * x
		}
	}
	return
}
```

#### TypeScript

```ts
function sumOfSquares(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (n % (i + 1) === 0) {
            ans += nums[i] * nums[i];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
