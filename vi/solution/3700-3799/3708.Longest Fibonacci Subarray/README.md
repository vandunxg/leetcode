---
comments: true
difficulty: Medium
rating: 1380
source: Biweekly Contest 167 Q2
tags:
    - Array
---

<!-- problem:start -->

# [3708. Longest Fibonacci Subarray](https://leetcode.com/problems/longest-fibonacci-subarray)

[中文文档](/solution/3700-3799/3708.Longest%20Fibonacci%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các số nguyên <strong>dương</strong> <code>nums</code>.</p>

<p>Một mảng <strong>Fibonacci</strong> là một dãy liên tiếp mà số hạng thứ ba và các số hạng sau đó đều bằng tổng của hai số hạng liền trước.</p>

<p>Hãy trả về độ dài của <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> <strong>Fibonacci</strong> dài nhất trong <code>nums</code>.</p>

<p><strong>Lưu ý:</strong> Các mảng con có độ dài 1 hoặc 2 luôn là <strong>Fibonacci</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1,2,3,5,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con Fibonacci dài nhất là <code>nums[2..6] = [1, 1, 2, 3, 5]</code>.</p>

<p><code>[1, 1, 2, 3, 5]</code> là Fibonacci vì <code>1 + 1 = 2</code>, <code>1 + 2 = 3</code> và <code>2 + 3 = 5</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,2,7,9,16]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con Fibonacci dài nhất là <code>nums[0..4] = [5, 2, 7, 9, 16]</code>.</p>

<p><code>[5, 2, 7, 9, 16]</code> là Fibonacci vì <code>5 + 2 = 7</code>, <code>2 + 7 = 9</code> và <code>7 + 9 = 16</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1000000000,1000000000,1000000000]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con Fibonacci dài nhất là <code>nums[1..2] = [1000000000, 1000000000]</code>.</p>

<p><code>[1000000000, 1000000000]</code> là Fibonacci vì độ dài của nó là 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc Fibonacci chỉ xét ba phần tử liên tiếp, vì vậy khi quy tắc bị phá vỡ, ta chỉ cần bắt đầu lại thay vì duyệt ngược từ mọi điểm kết thúc bên phải. Một biến duy nhất lưu độ dài của dãy kết thúc tại chỉ số hiện tại: tăng độ dài khi công thức truy hồi đúng, nếu không thì đặt lại thành $2$ (bất kỳ cặp phần tử nào cũng hợp lệ).

<!-- thinking:end -->

Ta có thể dùng một biến $f$ để ghi nhận độ dài của mảng con Fibonacci dài nhất kết thúc tại phần tử hiện tại. Ban đầu, $f=2$, vì bất kỳ hai phần tử nào cũng có thể tạo thành một mảng con Fibonacci.

Sau đó, ta duyệt mảng từ chỉ số $2$. Với mỗi phần tử $nums[i]$, nếu nó bằng tổng của hai phần tử trước đó, tức là $nums[i] = nums[i-1] + nums[i-2]$, điều đó có nghĩa là phần tử hiện tại có thể được nối vào mảng con Fibonacci trước đó, nên ta tăng $f$ thêm $1$. Ngược lại, phần tử hiện tại không thể được nối vào mảng con Fibonacci trước đó, nên ta đặt lại $f$ thành $2$. Trong quá trình duyệt, ta liên tục cập nhật đáp án $\textit{ans} = \max(\textit{ans}, f)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        n = len(nums)
        ans = f = 2
        for i in range(2, n):
            if nums[i] == nums[i - 1] + nums[i - 2]:
                f = f + 1
                ans = max(ans, f)
            else:
                f = 2
        return ans
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int f = 2;
        int ans = f;
        for (int i = 2; i < nums.length; ++i) {
            if (nums[i] == nums[i - 1] + nums[i - 2]) {
                ++f;
                ans = Math.max(ans, f);
            } else {
                f = 2;
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
    int longestSubarray(vector<int>& nums) {
        int f = 2;
        int ans = f;
        for (int i = 2; i < nums.size(); ++i) {
            if (nums[i] == nums[i - 1] + nums[i - 2]) {
                ++f;
                ans = max(ans, f);
            } else {
                f = 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubarray(nums []int) int {
	f := 2
	ans := f
	for i := 2; i < len(nums); i++ {
		if nums[i] == nums[i-1]+nums[i-2] {
			f++
			ans = max(ans, f)
		} else {
			f = 2
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[]): number {
    let f = 2;
    let ans = f;
    for (let i = 2; i < nums.length; ++i) {
        if (nums[i] === nums[i - 1] + nums[i - 2]) {
            ans = Math.max(ans, ++f);
        } else {
            f = 2;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
