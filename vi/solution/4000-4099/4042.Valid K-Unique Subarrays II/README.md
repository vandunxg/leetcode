---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [4042. Valid K-Unique Subarrays II 🔒](https://leetcode.com/problems/valid-k-unique-subarrays-ii)

[中文文档](/solution/4000-4099/4042.Valid%20K-Unique%20Subarrays%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Bạn cũng được cho các số nguyên <code>l0</code> và <code>r0</code>, xác định truy vấn đầu tiên, cùng một số nguyên <code>q</code>, biểu diễn tổng số truy vấn cần xử lý.</p>

<p>Một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> được xem là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li>Nó chứa <strong>đúng</strong> <code>k</code> số <strong>phân biệt</strong>, và</li>
	<li>Mỗi số phân biệt trong đó xuất hiện một số lần <strong>chẵn</strong>.</li>
</ul>

<p>Với truy vấn 0, đặt <code>l<sub>0</sub> = l0</code> và <code>r<sub>0</sub> = r0</code>.</p>

<p>Gọi <code>ans<sub>i</sub></code> là kết quả của truy vấn thứ <code>i<sup>th</sup></code>, trong đó <code>ans<sub>i</sub> = 1</code> nếu <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> là <strong>hợp lệ</strong>, và <code>ans<sub>i</sub> = 0</code> nếu ngược lại.</p>

<p>Với mỗi <code>i &gt; 0</code>, tạo truy vấn tiếp theo như sau:</p>

<ul>
	<li>Nếu <code>ans<sub>i-1</sub> = 1</code>, đặt <code>g<sub>i-1</sub> = l<sub>i-1</sub> + r<sub>i-1</sub></code>. Ngược lại, đặt <code>g<sub>i-1</sub> = r<sub>i-1</sub> - l<sub>i-1</sub></code>.</li>
	<li>Tính <code>l<sub>i</sub> = (l<sub>i-1</sub> XOR g<sub>i-1</sub>) % n</code> và <code>r<sub>i</sub> = (r<sub>i-1</sub> XOR g<sub>i-1</sub>) % n</code>.</li>
	<li>Nếu <code>l<sub>i</sub> &gt; r<sub>i</sub></code>, đổi chỗ chúng.</li>
</ul>

<p>Trả về một mảng boolean <code>ans</code>, trong đó <code>ans[i]</code> là <code>true</code> nếu <code>ans<sub>i</sub> = 1</code>, và <code>false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,1], k = 2, l0 = 1, r0 = 2, q = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[false,true]</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th><code>i</code></th>
			<th><code>[l<sub>i</sub>, r<sub>i</sub>]</code></th>
			<th>Mảng con</th>
			<th>Các số phân biệt</th>
			<th>Số lần xuất hiện</th>
			<th>Kiểm tra tính hợp lệ</th>
			<th><code>ans[i]</code></th>
			<th><code>[l<sub>i+1</sub>, r<sub>i+1</sub>]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>0</td>
			<td>[1, 2]</td>
			<td>[2, 2]</td>
			<td>{2} &rarr; 1</td>
			<td>{2:2}</td>
			<td><code>false</code>: Mảng con chứa ít hơn <code>k</code> số phân biệt.</td>
			<td><code>ans<sub>0</sub> = 0</code></td>
			<td><code>g<sub>0</sub> = 2 - 1 = 1<br />
			l<sub>1</sub> = (1 XOR 1) % 4 = 0<br />
			r<sub>1</sub> = (2 XOR 1) % 4 = 3</code></td>
		</tr>
		<tr>
			<td>1</td>
			<td>[0, 3]</td>
			<td>[1, 2, 2, 1]</td>
			<td>{1,2} &rarr; 2</td>
			<td>{1:2,2:2}</td>
			<td><code>true</code>: Mảng con chứa đúng <code>k</code> số phân biệt, mỗi số xuất hiện một số lần chẵn.</td>
			<td><code>ans<sub>1</sub> = 1</code></td>
			<td>-</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [false, true]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,3,4], k = 1, l0 = 2, r0 = 3, q = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,false]</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th><code>i</code></th>
			<th><code>[l<sub>i</sub>, r<sub>i</sub>]</code></th>
			<th>Mảng con</th>
			<th>Các số phân biệt</th>
			<th>Số lần xuất hiện</th>
			<th>Kiểm tra tính hợp lệ</th>
			<th><code>ans[i]</code></th>
			<th><code>[l<sub>i+1</sub>, r<sub>i+1</sub>]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>0</td>
			<td>[2, 3]</td>
			<td>[3, 3]</td>
			<td>{3} &rarr; 1</td>
			<td>{3:2}</td>
			<td><code>true</code>: Mảng con chứa đúng <code>k</code> số phân biệt, mỗi số xuất hiện một số lần chẵn.</td>
			<td><code>ans<sub>0</sub> = 1</code></td>
			<td><code>g<sub>0</sub> = 2 + 3 = 5<br />
			l<sub>1</sub> = (2 XOR 5) % 5 = 7 % 5 = 2<br />
			r<sub>1</sub> = (3 XOR 5) % 5 = 6 % 5 = 1</code><br />
			Vì <code>l<sub>1</sub> &gt; r<sub>1</sub></code>, đổi chỗ chúng để nhận được <code>[l<sub>1</sub>, r<sub>1</sub>] = [1, 2]</code>.</td>
		</tr>
		<tr>
			<td>1</td>
			<td>[1, 2]</td>
			<td>[2, 3]</td>
			<td>{2,3} &rarr; 2</td>
			<td>{2:1,3:1}</td>
			<td><code>false</code>: Mảng con chứa 2 số phân biệt thay vì đúng <code>k = 1</code>.</td>
			<td><code>ans<sub>1</sub> = 0</code></td>
			<td>-</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [true, false]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 5 &times; 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 5 &times; 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li><code>0 &lt;= l<sub>0</sub> &lt; r<sub>0</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= q &lt;= 5 &times; 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện kiểm tra giống phần I, nhưng các truy vấn được tạo trực tuyến từ đáp án trước đó, và cả $n$ lẫn $q$ đều có thể đạt $5\times 10^5$, nên ngay cả các hệ số log lớn cũng khiến giới hạn trở nên chặt chẽ.
>
> Các truy vấn được tạo không thể sắp xếp lại để xử lý offline. Khi mọi tần suất đều chẵn, ta vẫn có thể dùng prefix XOR-hash sau khi tiền xử lý tuyến tính; phần đếm số lượng phần tử phân biệt sử dụng các danh sách vị trí xuất hiện hoặc các cấu trúc prefix theo từng giá trị.
>
> Mỗi lần kiểm tra nên có thời gian hằng số hoặc một logarithm rất ngắn, sau đó các đầu mút tiếp theo được suy ra theo quy tắc.

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
