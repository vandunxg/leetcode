---
comments: true
difficulty: Hard
rating: 2314
source: Weekly Contest 516 Q4
---

<!-- problem:start -->

# [4033. Valid K-Unique Subarrays I](https://leetcode.com/problems/valid-k-unique-subarrays-i)

[中文文档](/solution/4000-4099/4033.Valid%20K-Unique%20Subarrays%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2D <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> biểu diễn <span data-keyword="subarray-nonempty"><strong>mảng con</strong></span> <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.</p>

<p>Với mỗi truy vấn, <strong>mảng con</strong> <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> được xem là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li>Nó chứa <strong>đúng</strong> <code>k</code> số <strong>phân biệt</strong>, và</li>
	<li><span data-keyword="frequency-array"><strong>tần suất</strong></span> của mọi số trong <strong>mảng con</strong> đều là <strong>số chẵn</strong>.</li>
</ul>

<p>Trả về một mảng boolean <code>ans</code>, trong đó <code>ans[i]</code> là <code>true</code> nếu <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> <strong>hợp lệ</strong>, và <code>false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,1], k = 2, queries = [[0,1],[0,3],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[false,true,false]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>[l<sub>i</sub>, r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Các số phân biệt</th>
			<th style="border: 1px solid black;">Tần suất</th>
			<th style="border: 1px solid black;">Kiểm tra tính hợp lệ</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[0, 1]</td>
			<td style="border: 1px solid black;">[1, 2]</td>
			<td style="border: 1px solid black;">{1, 2} &rarr; 2</td>
			<td style="border: 1px solid black;">{1: 1, 2: 1}</td>
			<td style="border: 1px solid black;"><code>false</code>: Số lần xuất hiện của các phần tử không phải là số chẵn.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[0, 3]</td>
			<td style="border: 1px solid black;">[1, 2, 2, 1]</td>
			<td style="border: 1px solid black;">{1, 2} &rarr; 2</td>
			<td style="border: 1px solid black;">{1: 2, 2: 2}</td>
			<td style="border: 1px solid black;"><code>true</code>: Có đúng <code>k = 2</code> phần tử phân biệt và tất cả đều xuất hiện số lần chẵn.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[1, 2]</td>
			<td style="border: 1px solid black;">[2, 2]</td>
			<td style="border: 1px solid black;">{2} &rarr; 1</td>
			<td style="border: 1px solid black;">{2: 2}</td>
			<td style="border: 1px solid black;"><code>false</code>: Số lượng phần tử phân biệt nhỏ hơn <code>k = 2</code>.</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [false, true, false]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,3,3], k = 1, queries = [[1,2],[0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,false]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>[l<sub>i</sub>, r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Các số phân biệt</th>
			<th style="border: 1px solid black;">Tần suất</th>
			<th style="border: 1px solid black;">Kiểm tra tính hợp lệ</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[1, 2]</td>
			<td style="border: 1px solid black;">[3, 3]</td>
			<td style="border: 1px solid black;">{3} &rarr; 1</td>
			<td style="border: 1px solid black;">{3: 2}</td>
			<td style="border: 1px solid black;"><code>true</code>: Có đúng <code>k = 1</code> phần tử phân biệt và phần tử này xuất hiện số lần chẵn.</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[0, 2]</td>
			<td style="border: 1px solid black;">[3, 3, 3]</td>
			<td style="border: 1px solid black;">{3} &rarr; 1</td>
			<td style="border: 1px solid black;">{3: 3}</td>
			<td style="border: 1px solid black;"><code>false</code>: 3 không xuất hiện số lần chẵn.</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [true, false]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i] == [l<sub>i</sub>, r<sub>i</sub>]</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt; r<sub>i</sub> &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn hỏi liệu một mảng con có đúng $k$ giá trị phân biệt và các tần suất đều là số chẵn hay không. Vì cả $n$ và số lượng truy vấn đều là $10^5$, ta không thể duyệt từng khoảng.
>
> Khi mọi tần suất đều chẵn, mỗi giá trị xuất hiện một số lần chẵn, có thể kiểm tra bằng prefix XOR-hash trong $O(1)$; ràng buộc về số lượng phần tử phân biệt cần thêm một prefix hoặc một cấu trúc đếm khác.
>
> Kết hợp hai điều kiện, ta có thể trả lời mỗi truy vấn trong thời gian logarithmic hoặc gần như hằng số.

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
