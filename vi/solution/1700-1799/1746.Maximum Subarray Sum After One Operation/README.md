---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1746. Maximum Subarray Sum After One Operation 🔒](https://leetcode.com/problems/maximum-subarray-sum-after-one-operation)

[中文文档](/solution/1700-1799/1746.Maximum%20Subarray%20Sum%20After%20One%20Operation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Bạn phải thực hiện <strong>chính xác một</strong> thao tác, trong đó có thể <strong>thay thế</strong> một phần tử <code>nums[i]</code> bằng <code>nums[i] * nums[i]</code>.&nbsp;</p>

<p>Trả về <em>tổng mảng con <strong>lớn nhất</strong> có thể đạt được sau <strong>chính xác một</strong> thao tác</em>. Mảng con phải không rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,-1,-4,-3]
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong> Bạn có thể thực hiện thao tác tại chỉ số 2 (đánh chỉ số từ 0) để được nums = [2,-1,<strong>16</strong>,-3]. Khi đó, tổng mảng con lớn nhất là 2 + -1 + 16 = 17.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-1,1,1,-1,-1,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Bạn có thể thực hiện thao tác tại chỉ số 1 (đánh chỉ số từ 0) để được nums = [1,<strong>1</strong>,1,1,-1,-1,1]. Khi đó, tổng mảng con lớn nhất là 1 + 1 + 1 + 1 = 4.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup>&nbsp;&lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phải bình phương đúng một phần tử, sau đó tìm tổng mảng con lớn nhất. Thử từng vị trí thay thế rồi chạy Kadane sẽ mất $O(n^2)$ và không đáp ứng được $n\le 10^5$.
>
> Với mảng con kết thúc tại chỉ số hiện tại, thao tác thay thế có thể chưa được dùng hoặc đã được dùng. Trường hợp đầu là Kadane thông thường; trường hợp sau là bình phương phần tử hiện tại sau một tiền tố chưa dùng thao tác, hoặc nối tiếp một tiền tố đã dùng thao tác.
>
> Hai giá trị cuốn chiếu $f,g$ theo dõi các trường hợp kết thúc đó; đáp án là giá trị lớn nhất trên toàn bộ mảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumAfterOperation(self, nums: List[int]) -> int:
        f = g = 0
        ans = -inf
        for x in nums:
            ff = max(f, 0) + x
            gg = max(max(f, 0) + x * x, g + x)
            f, g = ff, gg
            ans = max(ans, f, g)
        return ans
```

#### Java

```java
class Solution {
    public int maxSumAfterOperation(int[] nums) {
        int f = 0, g = 0;
        int ans = Integer.MIN_VALUE;
        for (int x : nums) {
            int ff = Math.max(f, 0) + x;
            int gg = Math.max(Math.max(f, 0) + x * x, g + x);
            f = ff;
            g = gg;
            ans = Math.max(ans, Math.max(f, g));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSumAfterOperation(vector<int>& nums) {
        int f = 0, g = 0;
        int ans = INT_MIN;
        for (int x : nums) {
            int ff = max(f, 0) + x;
            int gg = max(max(f, 0) + x * x, g + x);
            f = ff;
            g = gg;
            ans = max({ans, f, g});
        }
        return ans;
    }
};
```

#### Go

```go
func maxSumAfterOperation(nums []int) int {
	var f, g int
	ans := -(1 << 30)
	for _, x := range nums {
		ff := max(f, 0) + x
		gg := max(max(f, 0)+x*x, g+x)
		f, g = ff, gg
		ans = max(ans, max(f, g))
	}
	return ans
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn max_sum_after_operation(nums: Vec<i32>) -> i32 {
        // Here f[i] represents the value of max sub-array that ends with nums[i] with no substitution
        let mut f = 0;
        // g[i] represents the case with exact one substitution
        let mut g = 0;
        let mut ret = 1 << 31;

        // Begin the actual dp process
        for e in &nums {
            // f[i] = MAX(f[i - 1], 0) + nums[i]
            let new_f = std::cmp::max(f, 0) + *e;
            // g[i] = MAX(MAX(f[i - 1], 0) + nums[i] * nums[i], g[i - 1] + nums[i])
            let new_g = std::cmp::max(std::cmp::max(f, 0) + *e * *e, g + *e);
            // Update f[i] & g[i]
            f = new_f;
            g = new_g;
            // Since we start at 0, update answer after updating f[i] & g[i]
            ret = std::cmp::max(ret, g);
        }

        ret
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
