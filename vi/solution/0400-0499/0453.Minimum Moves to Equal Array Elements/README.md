---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [453. Minimum Moves to Equal Array Elements](https://leetcode.com/problems/minimum-moves-to-equal-array-elements)

[中文文档](/solution/0400-0499/0453.Minimum%20Moves%20to%20Equal%20Array%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có kích thước <code>n</code>, hãy trả về <em>số lượt di chuyển ít nhất cần thực hiện để mọi phần tử trong mảng bằng nhau</em>.</p>

<p>Trong mỗi lượt, bạn có thể tăng <code>n - 1</code> phần tử của mảng lên <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chỉ cần ba lượt (lưu ý mỗi lượt tăng hai phần tử):
[1,2,3]  =&gt;  [2,3,3]  =&gt;  [3,4,3]  =&gt;  [4,4,4]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>Đảm bảo đáp án nằm trong phạm vi số nguyên <strong>32-bit</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Tăng $n-1$ phần tử tương đương với giảm phần tử còn lại, cho đến khi mọi phần tử bằng giá trị nhỏ nhất. Mô phỏng các lượt tăng sẽ làm thay đổi toàn bộ mảng.
>
> Số lượt giảm cần thiết là $\sum nums - n\cdot\min(nums)$.
>
> Chỉ cần một lượt duyệt để tính giá trị nhỏ nhất và tổng; không cần thực hiện các thao tác thật sự.

<!-- thinking:end -->

Gọi giá trị nhỏ nhất của mảng $\textit{nums}$ là $\textit{mi}$, tổng các phần tử là $\textit{s}$ và độ dài mảng là $\textit{n}$.

Giả sử số lượt thao tác ít nhất là $\textit{k}$ và giá trị cuối cùng của mọi phần tử trong mảng là $\textit{x}$. Khi đó:

$$
\begin{aligned}
\textit{s} + (\textit{n} - 1) \times \textit{k} &= \textit{n} \times \textit{x} \\
\textit{x} &= \textit{mi} + \textit{k} \\
\end{aligned}
$$

Thay phương trình thứ hai vào phương trình thứ nhất, ta được:

$$
\begin{aligned}
\textit{s} + (\textit{n} - 1) \times \textit{k} &= \textit{n} \times (\textit{mi} + \textit{k}) \\
\textit{s} + (\textit{n} - 1) \times \textit{k} &= \textit{n} \times \textit{mi} + \textit{n} \times \textit{k} \\
\textit{k} &= \textit{s} - \textit{n} \times \textit{mi} \\
\end{aligned}
$$

Vậy số lượt thao tác ít nhất là $\textit{s} - \textit{n} \times \textit{mi}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, nums: List[int]) -> int:
        return sum(nums) - min(nums) * len(nums)
```

#### Java

```java
class Solution {
    public int minMoves(int[] nums) {
        return Arrays.stream(nums).sum() - Arrays.stream(nums).min().getAsInt() * nums.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(vector<int>& nums) {
        int s = 0;
        int mi = 1 << 30;
        for (int x : nums) {
            s += x;
            mi = min(mi, x);
        }
        return s - mi * nums.size();
    }
};
```

#### Go

```go
func minMoves(nums []int) int {
	mi := 1 << 30
	s := 0
	for _, x := range nums {
		s += x
		if x < mi {
			mi = x
		}
	}
	return s - mi*len(nums)
}
```

#### TypeScript

```ts
function minMoves(nums: number[]): number {
    let mi = 1 << 30;
    let s = 0;
    for (const x of nums) {
        s += x;
        mi = Math.min(mi, x);
    }
    return s - mi * nums.length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
