---
comments: true
difficulty: Medium
rating: 1328
source: Biweekly Contest 186 Q2
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3979. Maximum Valid Pair Sum](https://leetcode.com/problems/maximum-valid-pair-sum)

[中文文档](/solution/3900-3999/3979.Maximum%20Valid%20Pair%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Một cặp chỉ số <code>(i, j)</code> được gọi là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; n</code></li>
	<li><code>j - i &gt;= k</code></li>
</ul>

<p>Trả về giá trị <strong>lớn nhất</strong> của <code>nums[i] + nums[j]</code> trong tất cả các cặp hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,5,2,8], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp hợp lệ là:</p>

<ul>
	<li><code>(0, 2)</code>: <code>nums[0] + nums[2] = 6</code></li>
	<li><code>(0, 3)</code>: <code>nums[0] + nums[3] = 3</code></li>
	<li><code>(0, 4)</code>: <code>nums[0] + nums[4] = 9</code></li>
	<li><code>(1, 3)</code>: <code>nums[1] + nums[3] = 5</code></li>
	<li><code>(1, 4)</code>: <code>nums[1] + nums[4] = 11</code></li>
	<li><code>(2, 4)</code>: <code>nums[2] + nums[4] = 13</code></li>
</ul>

<p>Vì vậy, đáp án là 13.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,1,9], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì <code>k = 1</code>, mọi cặp đều hợp lệ.</li>
	<li>Giá trị lớn nhất đạt được với cặp <code>(0, 2)</code>, đó là <code>nums[0] + nums[2] = 5 + 9 = 14</code>.</li>
	<li>Vì vậy, đáp án là 14.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp hợp lệ cần khoảng cách giữa hai chỉ số ít nhất là $k$. Với đầu phải $j\ge k$, đầu trái không vượt quá $j-k$, nên ta chỉ cần giá trị lớn nhất trên $[0,j-k]$.
>
> Cửa sổ này mở rộng đơn điệu theo $j$: một biến $x$ liên tục nhận thêm $\textit{nums}[j-k]$ và $x+\textit{nums}[j]$ dùng để cập nhật đáp án.
>
> Ta duyệt trong $O(n)$ và không cần quét lại phía trái.

<!-- thinking:end -->

Với một cặp hợp lệ $(i, j)$, ta có $j - i \geq k$, tương đương $i \leq j - k$. Ta liệt kê đầu phải $j$ từ $k$. Với mỗi $j$, chỉ số lớn nhất của đầu trái là $j - k$. Ta duy trì giá trị lớn nhất $x$ của $\textit{nums}[i]$ trong đoạn $[0, j - k]$, rồi cập nhật đáp án bằng $x + \textit{nums}[j]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValidPairSum(self, nums: list[int], k: int) -> int:
        ans = x = 0
        for j in range(k, len(nums)):
            y = nums[j]
            x = max(x, nums[j - k])
            ans = max(ans, x + y)
        return ans
```

#### Java

```java
class Solution {
    public int maxValidPairSum(int[] nums, int k) {
        int ans = 0;
        int x = 0;
        for (int j = k; j < nums.length; ++j) {
            int y = nums[j];
            x = Math.max(x, nums[j - k]);
            ans = Math.max(ans, x + y);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxValidPairSum(vector<int>& nums, int k) {
        int ans = 0;
        int x = 0;
        for (int j = k; j < nums.size(); ++j) {
            int y = nums[j];
            x = max(x, nums[j - k]);
            ans = max(ans, x + y);
        }
        return ans;
    }
};
```

#### Go

```go
func maxValidPairSum(nums []int, k int) int {
	var ans, x int
	for j := k; j < len(nums); j++ {
		y := nums[j]
		x = max(x, nums[j-k])
		ans = max(ans, x+y)
	}
	return ans
}
```

#### TypeScript

```ts
function maxValidPairSum(nums: number[], k: number): number {
    let [ans, x] = [0, 0];
    for (let j = k; j < nums.length; ++j) {
        const y = nums[j];
        x = Math.max(x, nums[j - k]);
        ans = Math.max(ans, x + y);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
