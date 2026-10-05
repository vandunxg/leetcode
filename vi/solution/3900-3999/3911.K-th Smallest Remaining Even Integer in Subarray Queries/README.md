---
comments: true
difficulty: Hard
rating: 2155
source: Biweekly Contest 181 Q4
---

<!-- problem:start -->

# [3911. K-th Smallest Remaining Even Integer in Subarray Queries](https://leetcode.com/problems/k-th-smallest-remaining-even-integer-in-subarray-queries)

[中文文档](/solution/3900-3999/3911.K-th%20Smallest%20Remaining%20Even%20Integer%20in%20Subarray%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>, trong đó <code>nums</code> là mảng <strong><span data-keyword="strictly-increasing-array">tăng nghiêm ngặt</span></strong>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2D <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>]</code>.</p>

<p>Với mỗi truy vấn <code>[l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>]</code>:</p>

<ul>
	<li>Xét <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code></li>
	<li>Từ dãy <strong>vô hạn</strong> gồm tất cả <strong>số nguyên dương chẵn</strong>: <code>2, 4, 6, 8, 10, 12, 14, ...</code></li>
	<li><strong>Xóa</strong> tất cả phần tử xuất hiện trong <strong>mảng con</strong> <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.</li>
	<li>Tìm <strong>số nguyên nhỏ thứ</strong> <code>k<sub>i</sub><sup>th</sup></code> còn lại trong dãy sau khi xóa.</li>
</ul>

<p>Trả về một mảng số nguyên <code>ans</code>, trong đó <code>ans[i]</code> là kết quả của truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,7], queries = [[0,2,1],[1,1,2],[0,0,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,6,6]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>queries[i]</code></th>
			<th style="border: 1px solid black;"><code>nums[l<sub>i</sub>..r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Các số chẵn<br />
			bị xóa</th>
			<th style="border: 1px solid black;">Các số chẵn<br />
			còn lại</th>
			<th style="border: 1px solid black;"><code>k<sub>i</sub></code></th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[0, 2, 1]</td>
			<td style="border: 1px solid black;">[1, 4, 7]</td>
			<td style="border: 1px solid black;">[4]</td>
			<td style="border: 1px solid black;">2, 6, 8, ...</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[1, 1, 2]</td>
			<td style="border: 1px solid black;">[4]</td>
			<td style="border: 1px solid black;">[4]</td>
			<td style="border: 1px solid black;">2, 6, 8, ...</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">6</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[0, 0, 3]</td>
			<td style="border: 1px solid black;">[1]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">2, 4, 6, ...</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">6</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [2, 6, 6]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,8], queries = [[0,1,2],[1,2,1],[0,2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,2,12]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>queries[i]</code></th>
			<th style="border: 1px solid black;"><code>nums[l<sub>i</sub>..r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Các số chẵn<br />
			bị xóa</th>
			<th style="border: 1px solid black;">Các số chẵn<br />
			còn lại</th>
			<th style="border: 1px solid black;"><code>k<sub>i</sub></code></th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[0, 1, 2]</td>
			<td style="border: 1px solid black;">[2, 5]</td>
			<td style="border: 1px solid black;">[2]</td>
			<td style="border: 1px solid black;">4, 6, 8, ...</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">6</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[1, 2, 1]</td>
			<td style="border: 1px solid black;">[5, 8]</td>
			<td style="border: 1px solid black;">[8]</td>
			<td style="border: 1px solid black;">2, 4, 6, ...</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[0, 2, 4]</td>
			<td style="border: 1px solid black;">[2, 5, 8]</td>
			<td style="border: 1px solid black;">[2, 8]</td>
			<td style="border: 1px solid black;">4, 6, 10, 12, ...</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">12</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [6, 2, 12]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,6], queries = [[0,1,1],[1,1,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,8]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>queries[i]</code></th>
			<th style="border: 1px solid black;"><code>nums[l<sub>i</sub>..r<sub>i</sub>]</code></th>
			<th style="border: 1px solid black;">Các số chẵn<br />
			bị xóa</th>
			<th style="border: 1px solid black;">Các số chẵn<br />
			còn lại</th>
			<th style="border: 1px solid black;"><code>k<sub>i</sub></code></th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">[0, 1, 1]</td>
			<td style="border: 1px solid black;">[3, 6]</td>
			<td style="border: 1px solid black;">[6]</td>
			<td style="border: 1px solid black;">2, 4, 8, ...</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[1, 1, 3]</td>
			<td style="border: 1px solid black;">[6]</td>
			<td style="border: 1px solid black;">[6]</td>
			<td style="border: 1px solid black;">2, 4, 8, ...</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">8</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [2, 8]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>nums</code> là mảng tăng nghiêm ngặt</li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>]</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; nums.length</code></li>
	<li><code>1 &lt;= k<sub>i</sub> &lt;= 10<sup>9</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê các số chẵn dương rồi kiểm tra xem chúng có thuộc mảng con của từng truy vấn hay không sẽ thất bại với $n,q\le 10^5$ và $k_i$ có thể lên tới $10^9$. Vì $\textit{nums}$ tăng nghiêm ngặt, các số chẵn bị xóa trong một mảng con tạo thành một tập hợp liên tiếp theo thứ tự.
>
> Số chẵn dương còn lại thứ $k$ chính là $2k$ được dịch sang phải theo số lượng số chẵn bị xóa không lớn hơn ứng viên đó. Vì vậy, bài toán được đưa về việc đếm các số chẵn trong một đoạn chỉ số rồi điều chỉnh thứ hạng tương ứng.
>
> Thư mục này hiện chưa có lời giải được cài đặt, nên phần trình bày dừng lại ở bước biến đổi đó; mọi cấu trúc cụ thể đều phải trả lời các truy vấn đếm trên đoạn với thời gian gần logarit.

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
