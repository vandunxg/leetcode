---
comments: true
difficulty: Hard
tags:
    - Stack
    - Recursion
    - Hash Table
    - Math
    - String
---

<!-- problem:start -->

# [770. Basic Calculator IV](https://leetcode.com/problems/basic-calculator-iv)

[中文文档](/solution/0700-0799/0770.Basic%20Calculator%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một biểu thức như <code>expression = &quot;e + 8 - a + 5&quot;</code> và một map giá trị như <code>{&quot;e&quot;: 1}</code> (được biểu diễn bằng <code>evalvars = [&quot;e&quot;]</code> và <code>evalints = [1]</code>), hãy trả về danh sách token biểu diễn biểu thức đã rút gọn, chẳng hạn <code>[&quot;-1*a&quot;,&quot;14&quot;]</code>.</p>

<ul>
	<li>Biểu thức gồm các đoạn và ký hiệu xen kẽ nhau, mỗi đoạn và ký hiệu được ngăn cách bằng một dấu cách.</li>
	<li>Một đoạn là biểu thức trong ngoặc đơn, một biến hoặc một số nguyên không âm.</li>
	<li>Biến là chuỗi gồm các chữ cái thường (không có chữ số). Biến có thể gồm nhiều chữ cái và không bao giờ có hệ số đứng trước hoặc toán tử một ngôi như <code>&quot;2x&quot;</code> hay <code>&quot;-x&quot;</code>.</li>
</ul>

<p>Biểu thức được tính theo thứ tự thông thường: ngoặc trước, sau đó phép nhân, rồi phép cộng và phép trừ.</p>

<ul>
	<li>Ví dụ, <code>expression = &quot;1 + 2 * 3&quot;</code> có kết quả là <code>[&quot;7&quot;]</code>.</li>
</ul>

<p>Định dạng đầu ra như sau:</p>

<ul>
	<li>Với mỗi hạng tử chứa biến tự do và có hệ số khác 0, các biến tự do trong hạng tử được viết theo thứ tự từ điển.
	<ul>
		<li>Ví dụ, ta không viết hạng tử như <code>&quot;b*a*c&quot;</code>, mà chỉ viết <code>&quot;a*b*c&quot;</code>.</li>
	</ul>
	</li>
	<li>Bậc của một hạng tử bằng số biến tự do được nhân với nhau, tính cả số lần lặp. Các hạng tử bậc cao hơn được viết trước; nếu cùng bậc thì sắp xếp theo thứ tự từ điển, bỏ qua hệ số đứng đầu của hạng tử.
	<ul>
		<li>Ví dụ, <code>&quot;a*a*b*c&quot;</code> có bậc <code>4</code>.</li>
	</ul>
	</li>
	<li>Hệ số đứng đầu của hạng tử được đặt ngay bên trái, ngăn cách với các biến bằng dấu hoa thị (nếu có biến). Hệ số đứng đầu bằng 1 vẫn phải được ghi.</li>
	<li>Ví dụ về đáp án được định dạng đúng: <code>[&quot;-2*a*a*a&quot;, &quot;3*a*a*b&quot;, &quot;3*b*b&quot;, &quot;4*a&quot;, &quot;5*c&quot;, &quot;-6&quot;]</code>.</li>
	<li>Không đưa các hạng tử (kể cả hằng số) có hệ số <code>0</code> vào kết quả.
	<ul>
		<li>Ví dụ, biểu thức <code>&quot;0&quot;</code> có đầu ra là <code>[]</code>.</li>
	</ul>
	</li>
</ul>

<p><strong>Lưu ý:</strong> Có thể giả sử biểu thức đầu vào luôn hợp lệ. Mọi kết quả trung gian đều nằm trong phạm vi <code>[-2<sup>31</sup>, 2<sup>31</sup> - 1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;e + 8 - a + 5&quot;, evalvars = [&quot;e&quot;], evalints = [1]
<strong>Đầu ra:</strong> [&quot;-1*a&quot;,&quot;14&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;e - 8 + temperature - pressure&quot;, evalvars = [&quot;e&quot;, &quot;temperature&quot;], evalints = [1, 12]
<strong>Đầu ra:</strong> [&quot;-1*pressure&quot;,&quot;5&quot;]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;(e + 8) * (e - 8)&quot;, evalvars = [], evalints = []
<strong>Đầu ra:</strong> [&quot;1*e*e&quot;,&quot;-64&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= expression.length &lt;= 250</code></li>
	<li><code>expression</code> chỉ gồm chữ cái tiếng Anh viết thường, chữ số, <code>&#39;+&#39;</code>, <code>&#39;-&#39;</code>, <code>&#39;*&#39;</code>, <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code>, <code>&#39; &#39;</code>.</li>
	<li><code>expression</code> không có dấu cách ở đầu hoặc cuối.</li>
	<li>Tất cả token trong <code>expression</code> được ngăn cách bằng đúng một dấu cách.</li>
	<li><code>0 &lt;= evalvars.length &lt;= 100</code></li>
	<li><code>1 &lt;= evalvars[i].length &lt;= 20</code></li>
	<li><code>evalvars[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>evalints.length == evalvars.length</code></li>
	<li><code>-100 &lt;= evalints[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khai triển biểu thức có biến, các phép toán $+/-/*$ và ngoặc thành đa thức, đồng thời gộp các hạng tử đồng dạng. Các tab ngôn ngữ ở đây để trống; cách làm thông thường là biểu diễn và tính toán đa thức.
>
> Số và biến là các đơn thức; phép cộng gộp hệ số, còn phép nhân nối các đa tập biến. Dùng stack hoặc recursive descent để xử lý đúng thứ tự ưu tiên, đồng thời thay thế các biến đã biết.
>
> Xuất các hạng tử khác 0 theo thứ tự bậc giảm dần, sau đó theo danh sách biến từ điển.

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
