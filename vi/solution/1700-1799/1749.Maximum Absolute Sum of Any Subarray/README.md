---
comments: true
difficulty: Medium
rating: 1541
source: Biweekly Contest 45 Q2
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1749. Maximum Absolute Sum of Any Subarray](https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray)

[中文文档](/solution/1700-1799/1749.Maximum%20Absolute%20Sum%20of%20Any%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. <strong>Tổng tuyệt đối</strong> của mảng con <code>[nums<sub>l</sub>, nums<sub>l+1</sub>, ..., nums<sub>r-1</sub>, nums<sub>r</sub>]</code> là <code>abs(nums<sub>l</sub> + nums<sub>l+1</sub> + ... + nums<sub>r-1</sub> + nums<sub>r</sub>)</code>.</p>

<p>Trả về <em>tổng tuyệt đối <strong>lớn nhất</strong> của một mảng con <strong>(có thể rỗng)</strong> của </em><code>nums</code>.</p>

<p>Lưu ý rằng <code>abs(x)</code> được định nghĩa như sau:</p>

<ul>
<li>Nếu <code>x</code> là số nguyên âm thì <code>abs(x) = -x</code>.</li>
<li>Nếu <code>x</code> là số nguyên không âm thì <code>abs(x) = x</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-3,2,3,-4]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Mảng con [2,3] có tổng tuyệt đối = abs(2+3) = abs(5) = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,-5,1,-4,3,-2]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Mảng con [-5,1,-4] có tổng tuyệt đối = abs(-5+1-4) = abs(-8) = 8.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tổng tuyệt đối lớn nhất của mảng con là giá trị lớn hơn giữa tổng mảng con lớn nhất và trị tuyệt đối của tổng mảng con nhỏ nhất.
>
> Kadane theo dõi tổng lớn nhất và nhỏ nhất $f,g$ kết thúc tại đây; đáp án là giá trị lớn nhất trên toàn cục của $f$ và $|g|$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là giá trị lớn nhất của mảng con kết thúc tại $nums[i]$, và $g[i]$ là giá trị nhỏ nhất của mảng con kết thúc tại $nums[i]$. Công thức chuyển trạng thái của $f[i]$ và $g[i]$ như sau:

$$
\begin{aligned}
f[i] &= \max(f[i - 1], 0) + nums[i] \\
g[i] &= \min(g[i - 1], 0) + nums[i]
\end{aligned}
$$

Đáp án cuối cùng là giá trị lớn nhất của $max(f[i], |g[i]|)$.

Vì $f[i]$ và $g[i]$ chỉ liên quan đến $f[i - 1]$ và $g[i - 1]$, ta có thể dùng hai biến thay cho mảng, giảm độ phức tạp không gian xuống $O(1)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAbsoluteSum(self, nums: List[int]) -> int:
        f = g = 0
        ans = 0
        for x in nums:
            f = max(f, 0) + x
            g = min(g, 0) + x
            ans = max(ans, f, abs(g))
        return ans
```

#### Java

```java
class Solution {
    public int maxAbsoluteSum(int[] nums) {
        int f = 0, g = 0;
        int ans = 0;
        for (int x : nums) {
            f = Math.max(f, 0) + x;
            g = Math.min(g, 0) + x;
            ans = Math.max(ans, Math.max(f, Math.abs(g)));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxAbsoluteSum(vector<int>& nums) {
        int f = 0, g = 0;
        int ans = 0;
        for (int& x : nums) {
            f = max(f, 0) + x;
            g = min(g, 0) + x;
            ans = max({ans, f, abs(g)});
        }
        return ans;
    }
};
```

#### Go

```go
func maxAbsoluteSum(nums []int) (ans int) {
	var f, g int
	for _, x := range nums {
		f = max(f, 0) + x
		g = min(g, 0) + x
		ans = max(ans, max(f, abs(g)))
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function maxAbsoluteSum(nums: number[]): number {
    let f = 0;
    let g = 0;
    let ans = 0;
    for (const x of nums) {
        f = Math.max(f, 0) + x;
        g = Math.min(g, 0) + x;
        ans = Math.max(ans, f, -g);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_absolute_sum(nums: Vec<i32>) -> i32 {
        let mut f = 0;
        let mut g = 0;
        let mut ans = 0;
        for x in nums {
            f = i32::max(f, 0) + x;
            g = i32::min(g, 0) + x;
            ans = i32::max(ans, f.max(-g));
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
