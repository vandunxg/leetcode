---
comments: true
difficulty: Medium
rating: 2028
source: Weekly Contest 433 Q2
tags:
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
    - Sorting
---

<!-- problem:start -->

# [3428. Maximum and Minimum Sums of at Most Size K Subsequences](https://leetcode.com/problems/maximum-and-minimum-sums-of-at-most-size-k-subsequences)

[中文文档](/solution/3400-3499/3428.Maximum%20and%20Minimum%20Sums%20of%20at%20Most%20Size%20K%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên dương <code>k</code>. Hãy trả về tổng của các phần tử <strong>lớn nhất</strong> và <strong>nhỏ nhất</strong> trong tất cả <strong><span data-keyword="subsequence-sequence-nonempty">dãy con</span></strong> của <code>nums</code> có <strong>nhiều nhất</strong> <code>k</code> phần tử.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> 24</p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con của <code>nums</code> có nhiều nhất 2 phần tử là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><b>Dãy con </b></th>
			<th style="border: 1px solid black;">Nhỏ nhất</th>
			<th style="border: 1px solid black;">Lớn nhất</th>
			<th style="border: 1px solid black;">Tổng</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2]</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[3]</code></td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">6</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 3]</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2, 3]</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">5</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><strong>Tổng cuối cùng</strong></td>
			<td style="border: 1px solid black;">&nbsp;</td>
			<td style="border: 1px solid black;">&nbsp;</td>
			<td style="border: 1px solid black;">24</td>
		</tr>
	</tbody>
</table>

<p>Kết quả là 24.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,0,6], k = 1</span></p>

<p><strong>Đầu ra:</strong> 2<span class="example-io">2</span></p>

<p><strong>Giải thích: </strong></p>

<p>Với các dãy con có đúng 1 phần tử, giá trị nhỏ nhất và lớn nhất chính là phần tử đó. Do đó, tổng là <code>5 + 5 + 0 + 0 + 6 + 6 = 22</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> 12</p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con <code>[1, 1]</code> và <code>[1]</code> mỗi loại xuất hiện 3 lần. Với tất cả các dãy con này, giá trị nhỏ nhất và lớn nhất đều bằng 1. Vì vậy, tổng là 12.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code><font face="monospace">1 &lt;= k &lt;= min(70, nums.length)</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính tổng các giá trị lớn nhất cộng với tổng các giá trị nhỏ nhất trên các dãy con có độ dài nhiều nhất $k$. Với $n\le 10^5$ và $k\le 100$, việc liệt kê các dãy con là không khả thi.
>
> Sau khi sắp xếp, số lần $a_i$ là giá trị lớn nhất (nhỏ nhất) chính là số cách chọn nhiều nhất $k-1$ phần tử từ bên trái (bên phải) của nó.
>
> Vì vậy, ta cộng $a_i\cdot\sum_{j=0}^{\min(i,k-1)}C(i,j)$ vào đóng góp của giá trị lớn nhất, đồng thời cộng đóng góp đối xứng cho giá trị nhỏ nhất. Các hệ số nhị thức được xây dựng lần lượt theo từng hàng vì $k$ chỉ bằng $100$.

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
