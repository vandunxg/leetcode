---
comments: true
difficulty: Hard
rating: 2647
source: Weekly Contest 465 Q4
tags:
    - Binary Indexed Tree
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3671. Sum of Beautiful Subsequences](https://leetcode.com/problems/sum-of-beautiful-subsequences)

[中文文档](/solution/3600-3699/3671.Sum%20of%20Beautiful%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Với mọi số nguyên <strong>dương</strong> <code>g</code>, ta định nghĩa <strong>độ đẹp</strong> của <code>g</code> là <strong>tích</strong> của <code>g</code> và số lượng <strong><span data-keyword="subsequence-array-nonempty">dãy con</span></strong> <strong>tăng chặt</strong> của <code>nums</code> có ước chung lớn nhất (GCD) đúng bằng <code>g</code>.</p>

<p>Trả về <strong>tổng</strong> các giá trị <strong>độ đẹp</strong> của mọi số nguyên <code>g</code> dương.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các dãy con tăng chặt và GCD tương ứng là:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Dãy con</th>
			<th style="border: 1px solid black;">GCD</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">[1]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[2]</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[3]</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[1,2]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[1,3]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[2,3]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[1,2,3]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Tính độ đẹp cho từng GCD:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">GCD</th>
			<th style="border: 1px solid black;">Số lượng dãy con</th>
			<th style="border: 1px solid black;">Độ đẹp (GCD &times; Số lượng)</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">1 &times; 5 = 5</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2 &times; 1 = 2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">3 &times; 1 = 3</td>
		</tr>
	</tbody>
</table>

<p>Tổng độ đẹp là <code>5 + 2 + 3 = 10</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các dãy con tăng chặt và GCD tương ứng là:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Dãy con</th>
			<th style="border: 1px solid black;">GCD</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">[4]</td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[6]</td>
			<td style="border: 1px solid black;">6</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[4,6]</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>

<p>Tính độ đẹp cho từng GCD:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">GCD</th>
			<th style="border: 1px solid black;">Số lượng dãy con</th>
			<th style="border: 1px solid black;">Độ đẹp (GCD &times; Số lượng)</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2 &times; 1 = 2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">4 &times; 1 = 4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">6 &times; 1 = 6</td>
		</tr>
	</tbody>
</table>

<p>Tổng độ đẹp là <code>2 + 4 + 6 = 12</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 7 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Độ đẹp của một dãy con thường là một hàm theo $\gcd$. Với $n\le 10^4$ và các giá trị không vượt quá $7\times 10^4$, ta đếm theo $\gcd=d$ rồi dùng phép bù.
>
> Với mỗi $d$, chạy DP Fenwick trên các bội của $d$ để thu được $g[d]$, tổng số dãy con có $\gcd$ là bội của $d$.
>
> Đặt $f[d]=g[d]-\sum_{t>1}f[td]$ để $f[d]$ là phần đóng góp có $\gcd$ chính xác. Đáp án là $\sum d\cdot f[d]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python

```

#### Java

```java

```

#### C++

```cpp

```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
