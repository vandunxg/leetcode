---
comments: true
difficulty: Medium
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self)

[中文文档](/solution/0200-0299/0238.Product%20of%20Array%20Except%20Self/README.md)

## Mô tả

<!-- description:start -->

<p>Với một mảng số nguyên <code>nums</code>, hãy trả về <em>một mảng</em> <code>answer</code> <em>sao cho</em> <code>answer[i]</code> <em>bằng tích của tất cả các phần tử trong</em> <code>nums</code> <em>ngoại trừ</em> <code>nums[i]</code>.</p>

<p>Tích của mọi prefix hoặc suffix của <code>nums</code> được <strong>đảm bảo</strong> vừa với một số nguyên <strong>32-bit</strong>.</p>

<p>Bạn phải viết một thuật toán chạy trong thời gian <code>O(n)</code> và không sử dụng phép chia.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> [24,12,8,6]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [-1,1,0,-3,3]
<strong>Đầu ra:</strong> [0,0,9,0,0]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-30 &lt;= nums[i] &lt;= 30</code></li>
	<li>Đầu vào được tạo sao cho <code>answer[i]</code> được <strong>đảm bảo</strong> vừa với một số nguyên <strong>32-bit</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong>&nbsp;Bạn có thể giải bài toán với độ phức tạp không gian phụ <code>O(1)</code> không? (Mảng đầu ra <strong>không</strong> được tính là không gian phụ khi phân tích độ phức tạp không gian.)</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Tích của các phần tử ngoại trừ $nums[i]$ bằng tích bên trái nhân với tích bên phải. Hai mảng phụ cho các tích prefix và suffix sẽ sử dụng nhiều hơn không gian hằng số.
>
> Ghi các tích prefix vào answer từ trái sang phải, sau đó nhân với suffix đang được tích lũy từ phải sang trái, chỉ sử dụng mảng đầu ra.

<!-- thinking:end -->

Chúng ta định nghĩa hai biến $\textit{left}$ và $\textit{right}$ lần lượt biểu diễn tích của tất cả các phần tử ở bên trái và bên phải phần tử hiện tại. Ban đầu, $\textit{left} = 1$ và $\textit{right} = 1$. Chúng ta định nghĩa một mảng đáp án $\textit{ans}$ có độ dài $n$.

Đầu tiên, chúng ta duyệt mảng từ trái sang phải. Với phần tử thứ $i$, chúng ta cập nhật $\textit{ans}[i]$ bằng $\textit{left}$, sau đó nhân $\textit{left}$ với $\textit{nums}[i]$.

Tiếp theo, chúng ta duyệt mảng từ phải sang trái. Với phần tử thứ $i$, chúng ta cập nhật $\textit{ans}[i]$ thành $\textit{ans}[i] \times \textit{right}$, sau đó nhân $\textit{right}$ với $\textit{nums}[i]$.

Sau khi duyệt xong, chúng ta trả về mảng đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Bỏ qua không gian mà mảng đáp án sử dụng, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [0] * n
        left = right = 1
        for i, x in enumerate(nums):
            ans[i] = left
            left *= x
        for i in range(n - 1, -1, -1):
            ans[i] *= right
            right *= nums[i]
        return ans
```

#### Java

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0, left = 1; i < n; ++i) {
            ans[i] = left;
            left *= nums[i];
        }
        for (int i = n - 1, right = 1; i >= 0; --i) {
            ans[i] *= right;
            right *= nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0, left = 1; i < n; ++i) {
            ans[i] = left;
            left *= nums[i];
        }
        for (int i = n - 1, right = 1; ~i; --i) {
            ans[i] *= right;
            right *= nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func productExceptSelf(nums []int) []int {
	n := len(nums)
	ans := make([]int, n)
	left, right := 1, 1
	for i, x := range nums {
		ans[i] = left
		left *= x
	}
	for i := n - 1; i >= 0; i-- {
		ans[i] *= right
		right *= nums[i]
	}
	return ans
}
```

#### TypeScript

```ts
function productExceptSelf(nums: number[]): number[] {
    const n = nums.length;
    const ans: number[] = new Array(n);
    for (let i = 0, left = 1; i < n; ++i) {
        ans[i] = left;
        left *= nums[i];
    }
    for (let i = n - 1, right = 1; i >= 0; --i) {
        ans[i] *= right;
        right *= nums[i];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn product_except_self(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut ans = vec![1; n];
        for i in 1..n {
            ans[i] = ans[i - 1] * nums[i - 1];
        }
        let mut r = 1;
        for i in (0..n).rev() {
            ans[i] *= r;
            r *= nums[i];
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var productExceptSelf = function (nums) {
    const n = nums.length;
    const ans = new Array(n);
    for (let i = 0, left = 1; i < n; ++i) {
        ans[i] = left;
        left *= nums[i];
    }
    for (let i = n - 1, right = 1; i >= 0; --i) {
        ans[i] *= right;
        right *= nums[i];
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int[] ProductExceptSelf(int[] nums) {
        int n = nums.Length;
        int[] ans = new int[n];
        for (int i = 0, left = 1; i < n; ++i) {
            ans[i] = left;
            left *= nums[i];
        }
        for (int i = n - 1, right = 1; i >= 0; --i) {
            ans[i] *= right;
            right *= nums[i];
        }
        return ans;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer[]
     */
    function productExceptSelf($nums) {
        $n = count($nums);
        $ans = [];
        for ($i = 0, $left = 1; $i < $n; ++$i) {
            $ans[$i] = $left;
            $left *= $nums[$i];
        }
        for ($i = $n - 1, $right = 1; $i >= 0; --$i) {
            $ans[$i] *= $right;
            $right *= $nums[$i];
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
