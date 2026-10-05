---
comments: true
difficulty: Hard
rating: 2497
source: Weekly Contest 478 Q4
tags:
    - Segment Tree
    - Array
    - Math
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [3762. Minimum Operations to Equalize Subarrays](https://leetcode.com/problems/minimum-operations-to-equalize-subarrays)

[中文文档](/solution/3700-3799/3762.Minimum%20Operations%20to%20Equalize%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Trong một phép toán, bạn có thể <strong>tăng hoặc giảm</strong> bất kỳ phần tử nào của <code>nums</code> đúng <strong>chính xác</strong> <code>k</code> đơn vị.</p>

<p>Bạn cũng được cho một mảng số nguyên 2D <code>queries</code>, trong đó mỗi <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.</p>

<p>Với mỗi truy vấn, hãy tìm số phép toán <strong>nhỏ nhất</strong> cần thực hiện để làm cho <strong>tất cả</strong> phần tử trong <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> <strong>bằng nhau</strong>. Nếu không thể thực hiện, câu trả lời cho truy vấn đó là <code>-1</code>.</p>

<p>Trả về một mảng <code>ans</code>, trong đó <code>ans[i]</code> là câu trả lời cho truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,7], k = 3, queries = [[0,1],[0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một tập hợp phép toán tối ưu:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>[l<sub>i</sub>, r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;"><code>nums[l<sub>i</sub>..r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Có thể thực hiện</th>
			<th style="border: 1px solid black;">Các phép toán</th>
			<th style="border: 1px solid black;">Kết quả cuối<br />
			<code>nums[l<sub>i</sub>..r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
	</tbody>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[0, 1]</td>
			<td style="border: 1px solid black;">[1, 4]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;"><code>nums[0] + k = 1 + 3 = 4 = nums[1]</code></td>
			<td style="border: 1px solid black;">[4, 4]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[0, 2]</td>
			<td style="border: 1px solid black;">[1, 4, 7]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;"><code>nums[0] + k = 1 + 3 = 4 = nums[1]<br />
			nums[2] - k = 7 - 3 = 4 = nums[1]</code></td>
			<td style="border: 1px solid black;">[4, 4, 4]</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [1, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4], k = 2, queries = [[0,2],[0,0],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một tập hợp phép toán tối ưu:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>[l<sub>i</sub>, r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;"><code>nums[l<sub>i</sub>..r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Có thể thực hiện</th>
			<th style="border: 1px solid black;">Các phép toán</th>
			<th style="border: 1px solid black;">Kết quả cuối<br />
			<code>nums[l<sub>i</sub>..r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[0, 2]</td>
			<td style="border: 1px solid black;">[1, 2, 4]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">-</td>
			<td style="border: 1px solid black;">[1, 2, 4]</td>
			<td style="border: 1px solid black;">-1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[0, 0]</td>
			<td style="border: 1px solid black;">[1]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">Đã bằng nhau</td>
			<td style="border: 1px solid black;">[1]</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[1, 2]</td>
			<td style="border: 1px solid black;">[2, 4]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;"><code>nums[1] + k = 2 + 2 = 4 = nums[2]</code></td>
			<td style="border: 1px solid black;">[4, 4]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [-1, 0, 1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 4 &times; 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code>​​​​​​​</li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 4 &times; 10<sup>4</sup></code></li>
	<li><code><sup>​​​​​​​</sup>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Việc cộng hoặc trừ $k$ có thể đưa một đoạn về cùng giá trị khi và chỉ khi mọi giá trị có cùng số dư modulo $k$; đích đến cần ít phép toán nhất là giá trị trung vị. Khi có nhiều truy vấn, ta nhóm các chỉ số theo số dư, rồi dùng tổng tiền tố trên các giá trị đã sắp xếp của từng nhóm để trả lời mỗi đoạn.

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
