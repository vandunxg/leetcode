---
comments: true
difficulty: Hard
rating: 2235
source: Weekly Contest 471 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Hash Table
    - Math
    - Counting
    - Number Theory
---

<!-- problem:start -->

# [3715. Sum of Perfect Square Ancestors](https://leetcode.com/problems/sum-of-perfect-square-ancestors)

[中文文档](/solution/3700-3799/3715.Sum%20of%20Perfect%20Square%20Ancestors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một cây vô hướng có gốc tại node 0 gồm <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng mảng 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh vô hướng giữa hai node <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>.</p>

<p>Cho thêm mảng số nguyên <code>nums</code>, trong đó <code>nums[i]</code> là số nguyên dương được gán cho node <code>i</code>.</p>

<p>Định nghĩa giá trị <code>t<sub>i</sub></code> là số lượng <strong>tổ tiên</strong> của node <code>i</code> sao cho tích <code>nums[i] * nums[ancestor]</code> là một <strong><span data-keyword="perfect-square">số chính phương</span></strong>.</p>

<p>Trả về tổng tất cả giá trị <code>t<sub>i</sub></code> với mọi node <code>i</code> trong phạm vi <code>[1, n - 1]</code>.</p>

<p><strong>Ghi chú</strong>:</p>

<ul>
	<li>Trong một cây có gốc, <strong>tổ tiên</strong> của node <code>i</code> là tất cả node trên đường đi từ node <code>i</code> đến node gốc 0, <strong>không bao gồm</strong> chính <code>i</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], nums = [2,8,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code><strong>i</strong></code></th>
			<th style="border: 1px solid black;"><strong>Tổ tiên</strong></th>
			<th style="border: 1px solid black;"><code><strong>nums[i] * nums[ancestor]</strong></code></th>
			<th style="border: 1px solid black;">Kiểm tra số chính phương</th>
			<th style="border: 1px solid black;"><code><strong>t<sub>i</sub></strong></code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[0]</td>
			<td style="border: 1px solid black;"><code>nums[1] * nums[0] = 8 * 2 = 16</code></td>
			<td style="border: 1px solid black;">16 là số chính phương</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[1, 0]</td>
			<td style="border: 1px solid black;"><code>nums[2] * nums[1] = 2 * 8 = 16</code><br />
			<code>nums[2] * nums[0] = 2 * 2 = 4</code></td>
			<td style="border: 1px solid black;">Cả 4 và 16 đều là số chính phương</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>

<p>Vậy tổng số cặp tổ tiên hợp lệ trên tất cả node không phải node gốc là <code>1 + 2 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[0,2]], nums = [1,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code><strong>i</strong></code></th>
			<th style="border: 1px solid black;"><strong>Tổ tiên</strong></th>
			<th style="border: 1px solid black;"><code><strong>nums[i] * nums[ancestor]</strong></code></th>
			<th style="border: 1px solid black;">Kiểm tra số chính phương</th>
			<th style="border: 1px solid black;"><code><strong>t<sub>i</sub></strong></code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[0]</td>
			<td style="border: 1px solid black;"><code>nums[1] * nums[0] = 2 * 1 = 2</code></td>
			<td style="border: 1px solid black;">2 <strong>không phải</strong> là số chính phương</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[0]</td>
			<td style="border: 1px solid black;"><code>nums[2] * nums[0] = 4 * 1 = 4</code></td>
			<td style="border: 1px solid black;">4 là số chính phương</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p data-end="996" data-start="929">Vậy tổng số cặp tổ tiên hợp lệ trên tất cả node không phải node gốc là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[0,2],[1,3]], nums = [1,2,9,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><strong>Tổ tiên</strong></th>
			<th style="border: 1px solid black;"><code><strong>nums[i] * nums[ancestor]</strong></code></th>
			<th style="border: 1px solid black;">Kiểm tra số chính phương</th>
			<th style="border: 1px solid black;"><code><strong>t<sub>i</sub></strong></code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[0]</td>
			<td style="border: 1px solid black;"><code>nums[1] * nums[0] = 2 * 1 = 2</code></td>
			<td style="border: 1px solid black;">2 <strong>không phải</strong> là số chính phương</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[0]</td>
			<td style="border: 1px solid black;"><code>nums[2] * nums[0] = 9 * 1 = 9</code></td>
			<td style="border: 1px solid black;">9 là số chính phương</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">[1, 0]</td>
			<td style="border: 1px solid black;"><code>nums[3] * nums[1] = 4 * 2 = 8</code><br />
			<code>nums[3] * nums[0] = 4 * 1 = 4</code></td>
			<td style="border: 1px solid black;">Chỉ 4 là số chính phương</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Vậy tổng số cặp tổ tiên hợp lệ trên tất cả node không phải node gốc là <code>0 + 1 + 1 = 2</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>nums.length == n</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^5$ khiến việc đi từ mọi node về node gốc là không khả thi. Tích $nums[i]\cdot nums[anc]$ là số chính phương khi và chỉ khi hai giá trị có cùng kernel không chứa bình phương. Một DFS duy trì map tần suất của các kernel trên đường đi từ gốc đến node, thêm khi đi vào và khôi phục khi đi ra.

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
