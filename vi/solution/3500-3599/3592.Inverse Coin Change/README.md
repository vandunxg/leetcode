---
comments: true
difficulty: Medium
rating: 1700
source: Weekly Contest 455 Q2
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3592. Inverse Coin Change](https://leetcode.com/problems/inverse-coin-change)

[中文文档](/solution/3500-3599/3592.Inverse%20Coin%20Change/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 1</strong> <code>numWays</code>, trong đó <code>numWays[i]</code> biểu thị số cách chọn tổng bằng <code>i</code> bằng cách sử dụng nguồn cung cấp <strong>vô hạn</strong> của một số mệnh giá xu <em>cố định</em>. Mỗi mệnh giá là một số nguyên <strong>dương</strong> và có giá trị <strong>không vượt quá</strong> <code>numWays.length</code>.</p>

<p>Tuy nhiên, các mệnh giá xu chính xác đã bị <em>thất lạc</em>. Nhiệm vụ của bạn là khôi phục tập mệnh giá có thể tạo ra mảng <code>numWays</code> đã cho.</p>

<p>Trả về một mảng <strong>đã sắp xếp</strong> gồm các số nguyên <strong>không trùng lặp</strong>, biểu thị tập mệnh giá này.</p>

<p>Nếu không tồn tại tập mệnh giá nào như vậy, trả về một mảng <strong>rỗng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">numWays = [0,1,0,2,0,3,0,4,0,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,4,6]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Số tiền</th>
			<th style="border: 1px solid black;">Số cách</th>
			<th style="border: 1px solid black;">Giải thích</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">Không có cách nào chọn các đồng xu có tổng giá trị bằng 1.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Cách duy nhất là <code>[2]</code>.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">Không có cách nào chọn các đồng xu có tổng giá trị bằng 3.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Các cách là <code>[2, 2]</code> và <code>[4]</code>.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">Không có cách nào chọn các đồng xu có tổng giá trị bằng 5.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">Các cách là <code>[2, 2, 2]</code>, <code>[2, 4]</code> và <code>[6]</code>.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">Không có cách nào chọn các đồng xu có tổng giá trị bằng 7.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">8</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">Các cách là <code>[2, 2, 2, 2]</code>, <code>[2, 2, 4]</code>, <code>[2, 6]</code> và <code>[4, 4]</code>.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">9</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">Không có cách nào chọn các đồng xu có tổng giá trị bằng 9.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">10</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">Các cách là <code>[2, 2, 2, 2, 2]</code>, <code>[2, 2, 2, 4]</code>, <code>[2, 4, 4]</code>, <code>[2, 2, 6]</code> và <code>[4, 6]</code>.</td>
		</tr>
	</tbody>
</table>
<strong class="example">Ví dụ 2:</strong>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">numWays = [1,2,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,5]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Số tiền</th>
			<th style="border: 1px solid black;">Số cách</th>
			<th style="border: 1px solid black;">Giải thích</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Cách duy nhất là <code>[1]</code>.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Các cách là <code>[1, 1]</code> và <code>[2]</code>.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Các cách là <code>[1, 1, 1]</code> và <code>[1, 2]</code>.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">Các cách là <code>[1, 1, 1, 1]</code>, <code>[1, 1, 2]</code> và <code>[2, 2]</code>.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">Các cách là <code>[1, 1, 1, 1, 1]</code>, <code>[1, 1, 1, 2]</code>, <code>[1, 2, 2]</code> và <code>[5]</code>.</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">numWays = [1,2,3,4,15]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có tập mệnh giá nào thỏa mãn mảng này.</p>
</div>

<table style="border: 1px solid black;">
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= numWays.length &lt;= 100</code></li>
	<li><code>0 &lt;= numWays[i] &lt;= 2 * 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{numWays}[i]$ là số cách kết hợp không hạn chế có tổng bằng $i+1$ với các mệnh giá chưa biết. Duyệt các số tiền từ $1$ đến $n$: nếu số cách trong complete knapsack với các đồng xu đã tìm được ít hơn $\textit{numWays}$ đúng một cách, thì chính số tiền đó phải là một đồng xu mới; mọi chênh lệch khác đều là không thể.
>
> Khi chấp nhận một đồng xu, cập nhật các số tiền lớn hơn bằng công thức truy hồi của unbounded knapsack. Phép duyệt này cho ra danh sách tăng dần hoặc một mảng rỗng nếu có mâu thuẫn.

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
