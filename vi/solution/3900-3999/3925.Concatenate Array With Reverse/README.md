---
comments: true
difficulty: Easy
rating: 1176
source: Weekly Contest 501 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3925. Concatenate Array With Reverse](https://leetcode.com/problems/concatenate-array-with-reverse)

[中文文档](/solution/3900-3999/3925.Concatenate%20Array%20With%20Reverse/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Hãy xây dựng một mảng mới <code>ans</code> có độ dài <code>2 * n</code>, trong đó <code>n</code> phần tử đầu tiên giống với <code>nums</code>, còn <code>n</code> phần tử tiếp theo là các phần tử của <code>nums</code> theo thứ tự ngược lại.</p>

<p>Cụ thể, với <code>0 &lt;= i &lt;= n - 1</code>:</p>

<ul>
	<li><code>ans[i] = nums[i]</code></li>
	<li><code>ans[i + n] = nums[n - i - 1]</code></li>
</ul>

<p>Trả về mảng số nguyên <code>ans</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,3,3,2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>n</code> phần tử đầu tiên của <code>ans</code> giống với <code>nums</code>.</p>

<p>Với <code>n = 3</code> phần tử tiếp theo, mỗi phần tử được lấy từ <code>nums</code> theo thứ tự ngược lại:</p>

<ul>
	<li><code>ans[3] = nums[2] = 3</code></li>
	<li><code>ans[4] = nums[1] = 2</code></li>
	<li><code>ans[5] = nums[0] = 1</code></li>
</ul>

<p>Do đó, <code>ans = [1, 2, 3, 3, 2, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng không thay đổi khi đảo ngược. Do đó, <code>ans = [1, 1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 100$, ta chỉ cần cấp phát một mảng có độ dài $2n$. Nửa đầu sao chép $\textit{nums}$, còn nửa sau ghi các phần tử theo thứ tự ngược lại.
>
> Với mỗi $i$, ta đặt $\textit{ans}[i]=\textit{nums}[i]$ và $\textit{ans}[i+n]=\textit{nums}[n-i-1]$ trong một lần duyệt.

<!-- thinking:end -->

Ta tạo một mảng $\textit{ans}$ có độ dài $2 \times n$. $n$ phần tử đầu tiên giống với $\textit{nums}$, còn $n$ phần tử tiếp theo là các phần tử của $\textit{nums}$ theo thứ tự ngược lại.

Cụ thể, với $0 \leq i \leq n - 1$, ta đặt $\textit{ans}[i] = \textit{nums}[i]$ và $\textit{ans}[i + n] = \textit{nums}[n - i - 1]$.

Cuối cùng, trả về mảng $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def concatWithReverse(self, nums: list[int]) -> list[int]:
        n = len(nums)
        ans = [0] * (2 * n)
        for i, x in enumerate(nums):
            ans[i] = x
            ans[i + n] = nums[n - i - 1]
        return ans
```

#### Java

```java
class Solution {
    public int[] concatWithReverse(int[] nums) {
        int n = nums.length;
        int[] ans = new int[2 * n];
        for (int i = 0; i < n; ++i) {
            ans[i] = nums[i];
            ans[i + n] = nums[n - i - 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> concatWithReverse(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(2 * n);
        for (int i = 0; i < n; ++i) {
            ans[i] = nums[i];
            ans[i + n] = nums[n - i - 1];
        }
        return ans;
    }
};
```

#### Go

```go
func concatWithReverse(nums []int) []int {
	n := len(nums)
	ans := make([]int, 2*n)
	for i, x := range nums {
		ans[i] = x
		ans[i+n] = nums[n-i-1]
	}
	return ans
}
```

#### TypeScript

```ts
function concatWithReverse(nums: number[]): number[] {
    const n = nums.length;
    const ans: number[] = new Array(2 * n);
    for (let i = 0; i < n; ++i) {
        ans[i] = nums[i];
        ans[i + n] = nums[n - i - 1];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
