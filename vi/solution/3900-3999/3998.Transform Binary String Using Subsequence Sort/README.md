---
comments: true
difficulty: Medium
rating: 1862
source: Weekly Contest 511 Q3
---

<!-- problem:start -->

# [3998. Transform Binary String Using Subsequence Sort](https://leetcode.com/problems/transform-binary-string-using-subsequence-sort)

[中文文档](/solution/3900-3999/3998.Transform%20Binary%20String%20Using%20Subsequence%20Sort/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <span data-keyword="binary-string">chuỗi nhị phân</span> <code>s</code>.</p>

<p>Ta cũng có một mảng chuỗi <code>strs</code>, trong đó mỗi <code>strs[i]</code> có <strong>cùng</strong> độ dài với <code>s</code> và chỉ gồm các ký tự <code>&#39;0&#39;</code>, <code>&#39;1&#39;</code> và <code>&#39;?&#39;</code>. Mỗi <code>&#39;?&#39;</code> có thể được thay bằng <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</p>

<p>Ta có thể thực hiện thao tác sau bao nhiêu lần tùy ý (kể cả không lần nào):</p>

<ul>
	<li>Chọn một <span data-keyword="subsequence-string">dãy con</span> <code>sub</code> bất kỳ của <code>s</code>.</li>
	<li>Sắp xếp <code>sub</code> theo thứ tự <strong>không giảm</strong>.</li>
	<li>Thay <strong>dãy con</strong> đã chọn trong <code>s</code> bằng <code>sub</code> đã sắp xếp, giữ nguyên mọi ký tự khác.</li>
</ul>

<p>Trả về một mảng Boolean <code>ans</code>, trong đó <code>ans[i]</code> là <code>true</code> nếu có thể thay tất cả <code>&#39;?&#39;</code> trong <code>strs[i]</code> bằng <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code> rồi biến đổi <code>s</code> thành chuỗi thu được bằng các thao tác cho phép ở trên, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;101&quot;, strs = [&quot;1?1&quot;,&quot;0?1&quot;,&quot;0?0&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,true,false]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>strs[i]</code></th>
			<th style="border: 1px solid black;">Thay thế</th>
			<th style="border: 1px solid black;">Kết quả <code>strs[i]</code></th>
			<th style="border: 1px solid black;">Thao tác</th>
			<th style="border: 1px solid black;">Kết quả</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>&quot;1?1&quot;</code></td>
			<td style="border: 1px solid black;"><code>? &rarr; 0</code></td>
			<td style="border: 1px solid black;"><code>&quot;101&quot;</code></td>
			<td style="border: 1px solid black;">Trùng với <code>s</code>.</td>
			<td style="border: 1px solid black;"><code>true</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>&quot;0?1&quot;</code></td>
			<td style="border: 1px solid black;"><code>? &rarr; 1</code></td>
			<td style="border: 1px solid black;"><code>&quot;011&quot;</code></td>
			<td style="border: 1px solid black;">Chọn dãy con tại các chỉ số <code>[0..2]</code> của <code>s</code> &rarr; <code>&quot;101&quot;</code>.<br />
			Sắp xếp <code>&quot;101&quot;</code> để được <code>&quot;011&quot; = strs[i]</code>.</td>
			<td style="border: 1px solid black;"><code>true</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>&quot;0?0&quot;</code></td>
			<td style="border: 1px solid black;"><code>? &rarr; 0</code> hoặc <code>1</code></td>
			<td style="border: 1px solid black;"><code>&quot;000&quot;</code> hoặc <code>&quot;010&quot;</code></td>
			<td style="border: 1px solid black;">Không thể thực hiện.</td>
			<td style="border: 1px solid black;"><code>false</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy <code>ans = [true, true, false]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1100&quot;, strs = [&quot;0011&quot;,&quot;11?1&quot;,&quot;1?1?&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,false,true]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>strs[i]</code></th>
			<th style="border: 1px solid black;">Thay thế</th>
			<th style="border: 1px solid black;">Kết quả <code>strs[i]</code></th>
			<th style="border: 1px solid black;">Thao tác</th>
			<th style="border: 1px solid black;">Kết quả</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>&quot;0011&quot;</code></td>
			<td style="border: 1px solid black;">-</td>
			<td style="border: 1px solid black;"><code>&quot;0011&quot;</code></td>
			<td style="border: 1px solid black;">Chọn dãy con tại các chỉ số <code>[0..3]</code> của <code>s</code> &rarr; <code>&quot;1100&quot;</code>.<br />
			Sắp xếp <code>&quot;1100&quot;</code> để được <code>&quot;0011&quot; = strs[i]</code>.</td>
			<td style="border: 1px solid black;"><code>true</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>&quot;11?1&quot;</code></td>
			<td style="border: 1px solid black;"><code>? &rarr; 0</code></td>
			<td style="border: 1px solid black;"><code>&quot;1101&quot;</code></td>
			<td style="border: 1px solid black;">Không thể thực hiện.</td>
			<td style="border: 1px solid black;"><code>false</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>&quot;1?1?&quot;</code></td>
			<td style="border: 1px solid black;">Dấu đầu tiên <code>? &rarr; 0</code><br />
			Dấu thứ hai <code>? &rarr; 0</code></td>
			<td style="border: 1px solid black;"><code>&quot;1010&quot;</code></td>
			<td style="border: 1px solid black;">Chọn dãy con tại các chỉ số <code>[1, 2]</code> của <code>s</code> &rarr; <code>&quot;10&quot;</code>.<br />
			Sắp xếp <code>&quot;10&quot;</code> để được <code>&quot;01&quot;</code>, nên <code>s = &quot;1<u>01</u>0&quot;</code>.</td>
			<td style="border: 1px solid black;"><code>true</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy <code>ans = [true, false, true]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1010&quot;, strs = [&quot;0011&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>strs[i]</code></th>
			<th style="border: 1px solid black;">Thay thế</th>
			<th style="border: 1px solid black;">Kết quả <code>strs[i]</code></th>
			<th style="border: 1px solid black;">Thao tác</th>
			<th style="border: 1px solid black;">Kết quả</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>&quot;0011&quot;</code></td>
			<td style="border: 1px solid black;">-</td>
			<td style="border: 1px solid black;"><code>&quot;0011&quot;</code></td>
			<td style="border: 1px solid black;">Chọn dãy con tại các chỉ số <code>[0, 2, 3]</code> của <code>s</code> &rarr; <code>&quot;110&quot;</code>.<br />
			Sắp xếp <code>&quot;110&quot;</code> để được <code>&quot;011&quot;</code>, nên <code>s = &quot;0<u>0</u>11&quot; = strs[i]</code>.</td>
			<td style="border: 1px solid black;"><code>true</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy <code>ans = [true]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 2000</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= strs.length &lt;= 2000</code></li>
	<li><code>strs[i].length == n</code></li>
	<li><code>strs[i]</code> là <code>&#39;0&#39;</code>, <code>&#39;1&#39;</code> hoặc <code>&#39;?&#39;</code>​​​​​​​.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Việc sắp xếp một dãy con của chuỗi nhị phân chỉ dịch các số $0$ về bên trái trong các vị trí được chọn. Ký tự `?` trong mẫu có thể tự do chọn, vì vậy mỗi mẫu khả thi khi và chỉ khi có một cách điền có thể đạt được bằng cách liên tục dịch các số $0$ về trái trong $s$.
>
> Số lượng số $0$ phải phù hợp (ký tự `?` có thể linh hoạt), và không có số $1$ nào trong $s$ được nằm quá xa bên phải so với vị trí tương ứng của nó. Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng lại ở nhận xét “sắp xếp dãy con = dịch các số 0 sang trái, rồi khớp với mẫu”.

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
