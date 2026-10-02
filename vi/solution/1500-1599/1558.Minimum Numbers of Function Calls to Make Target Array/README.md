---
comments: true
difficulty: Medium
rating: 1637
source: Biweekly Contest 33 Q3
tags:
    - Greedy
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [1558. Minimum Numbers of Function Calls to Make Target Array](https://leetcode.com/problems/minimum-numbers-of-function-calls-to-make-target-array)

[中文文档](/solution/1500-1599/1558.Minimum%20Numbers%20of%20Function%20Calls%20to%20Make%20Target%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Ta có mảng số nguyên <code>arr</code> cùng độ dài, ban đầu mọi giá trị đều bằng <code>0</code>. Ta cũng có hàm <code>modify</code> sau:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1558.Minimum%20Numbers%20of%20Function%20Calls%20to%20Make%20Target%20Array/images/sample_2_1887.png" style="width: 573px; height: 294px;" />
<p>Ta muốn dùng hàm modify để chuyển <code>arr</code> thành <code>nums</code> với số lần gọi nhỏ nhất.</p>

<p>Trả về <em>số lần gọi hàm nhỏ nhất để tạo </em><code>nums</code><em> từ </em><code>arr</code>.</p>

<p>Các test case được tạo sao cho đáp án vừa trong số nguyên có dấu <strong>32-bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Tăng 1 (phần tử thứ hai): [0, 0] thành [0, 1] (1 phép toán).
Nhân đôi mọi phần tử: [0, 1] -&gt; [0, 2] -&gt; [0, 4] (2 phép toán).
Tăng 1 (cả hai phần tử)  [0, 4] -&gt; [1, 4] -&gt; <strong>[1, 5]</strong> (2 phép toán).
Tổng số phép toán: 1 + 2 + 2 = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Tăng 1 (cả hai phần tử) [0, 0] -&gt; [0, 1] -&gt; [1, 1] (2 phép toán).
Nhân đôi mọi phần tử: [1, 1] -&gt; <strong>[2, 2]</strong> (1 phép toán).
Tổng số phép toán: 2 + 1 = 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,5]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> (ban đầu)[0,0,0] -&gt; [1,0,0] -&gt; [1,0,1] -&gt; [2,0,2] -&gt; [2,1,2] -&gt; [4,2,4] -&gt; <strong>[4,2,5]</strong>(nums).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bắt đầu từ các số không; mỗi phép toán hoặc tăng một phần tử, hoặc nhân đôi toàn bộ mảng. $n\le 10^5$ và $nums[i]\le 10^9$, nên không thể mô phỏng từng giá trị. Nhân đôi tương đương dịch trái đồng thời; một phép tăng ghi một bit $1$ của một số.
>
> Mỗi $v$ cần $v.\mathrm{bit\_count}()$ phép tăng, còn số lần nhân đôi chung bằng độ dài bit của giá trị lớn nhất trừ một. Tổng của chúng là số lần gọi nhỏ nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        return sum(v.bit_count() for v in nums) + max(0, max(nums).bit_length() - 1)
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int ans = 0;
        int mx = 0;
        for (int v : nums) {
            mx = Math.max(mx, v);
            ans += Integer.bitCount(v);
        }
        ans += Integer.toBinaryString(mx).length() - 1;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        int ans = 0;
        int mx = 0;
        for (int v : nums) {
            mx = max(mx, v);
            ans += __builtin_popcount(v);
        }
        if (mx) ans += 31 - __builtin_clz(mx);
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	ans, mx := 0, 0
	for _, v := range nums {
		mx = max(mx, v)
		for v > 0 {
			ans += v & 1
			v >>= 1
		}
	}
	if mx > 0 {
		for mx > 0 {
			ans++
			mx >>= 1
		}
		ans--
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
