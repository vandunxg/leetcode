---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [396. Rotate Function](https://leetcode.com/problems/rotate-function)

[中文文档](/solution/0300-0399/0396.Rotate%20Function/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Gọi <code>arr<sub>k</sub></code> là mảng nhận được khi xoay <code>nums</code> theo chiều kim đồng hồ <code>k</code> vị trí. Ta định nghĩa <strong>hàm xoay</strong> <code>F</code> trên <code>nums</code> như sau:</p>

<ul>
	<li><code>F(k) = 0 * arr<sub>k</sub>[0] + 1 * arr<sub>k</sub>[1] + ... + (n - 1) * arr<sub>k</sub>[n - 1].</code></li>
</ul>

<p>Trả về <em>giá trị lớn nhất trong</em> <code>F(0), F(1), ..., F(n-1)</code>.</p>

<p>Test được tạo sao cho đáp án nằm trong phạm vi số nguyên <strong>32-bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,2,6]
<strong>Đầu ra:</strong> 26
<strong>Giải thích:</strong>
F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26
Vì vậy, giá trị lớn nhất trong F(0), F(1), F(2), F(3) là F(3) = 26.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [100]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $F(k)=\sum i\cdot \textit{nums}[(i+k)\bmod n]$; cần tìm giá trị lớn nhất của $F$. Tính lại cho từng phép xoay sẽ tốn $O(n^2)$. Có thể biểu diễn hiệu giữa hai giá trị $F$ liên tiếp bằng một công thức trực tiếp.
>
> $F(k+1)=F(k)+\sum nums-n\cdot nums[n-1-k]$. Tính $F(0)$, sau đó cập nhật lần lượt thêm $n-1$ lần và luôn giữ giá trị lớn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxRotateFunction(self, nums: List[int]) -> int:
        f = sum(i * v for i, v in enumerate(nums))
        n, s = len(nums), sum(nums)
        ans = f
        for i in range(1, n):
            f = f + s - n * nums[n - i]
            ans = max(ans, f)
        return ans
```

#### Java

```java
class Solution {
    public int maxRotateFunction(int[] nums) {
        int f = 0;
        int s = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            f += i * nums[i];
            s += nums[i];
        }
        int ans = f;
        for (int i = 1; i < n; ++i) {
            f = f + s - n * nums[n - i];
            ans = Math.max(ans, f);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxRotateFunction(vector<int>& nums) {
        int f = 0, s = 0, n = nums.size();
        for (int i = 0; i < n; ++i) {
            f += i * nums[i];
            s += nums[i];
        }
        int ans = f;
        for (int i = 1; i < n; ++i) {
            f = f + s - n * nums[n - i];
            ans = max(ans, f);
        }
        return ans;
    }
};
```

#### Go

```go
func maxRotateFunction(nums []int) int {
	f, s, n := 0, 0, len(nums)
	for i, v := range nums {
		f += i * v
		s += v
	}
	ans := f
	for i := 1; i < n; i++ {
		f = f + s - n*nums[n-i]
		ans = max(ans, f)
	}
	return ans
}
```

#### TypeScript

```ts
function maxRotateFunction(nums: number[]): number {
    const n = nums.length;
    const sum = nums.reduce((r, v) => r + v);
    let res = nums.reduce((r, v, i) => r + v * i, 0);
    let pre = res;
    for (let i = 1; i < n; i++) {
        pre = pre - (sum - nums[i - 1]) + nums[i - 1] * (n - 1);
        res = Math.max(res, pre);
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_rotate_function(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let sum: i32 = nums.iter().sum();
        let mut pre: i32 = nums.iter().enumerate().map(|(i, &v)| (i as i32) * v).sum();
        (0..n)
            .map(|i| {
                let res = pre;
                pre = pre - (sum - nums[i]) + nums[i] * ((n - 1) as i32);
                res
            })
            .max()
            .unwrap_or(0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
