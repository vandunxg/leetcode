---
comments: true
difficulty: Hard
tags:
    - Graph
    - Design
    - Topological Sort
    - Array
    - Hash Table
    - String
    - Matrix
---

<!-- problem:start -->

# [631. Design Excel Sum Formula 🔒](https://leetcode.com/problems/design-excel-sum-formula)

[中文文档](/solution/0600-0699/0631.Design%20Excel%20Sum%20Formula/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế các chức năng cơ bản của <strong>Excel</strong> và cài đặt công thức tính tổng.</p>

<p>Cài đặt class <code>Excel</code>:</p>

<ul>
	<li><code>Excel(int height, char width)</code> Khởi tạo object với <code>height</code> và <code>width</code> của bảng tính. Bảng tính là ma trận số nguyên <code>mat</code> kích thước <code>height x width</code>, có chỉ số hàng trong khoảng <code>[1, height]</code> và chỉ số cột trong khoảng <code>[&#39;A&#39;, width]</code>. Ban đầu, tất cả giá trị đều bằng <strong>0</strong>.</li>
	<li><code>void set(int row, char column, int val)</code> Thay đổi giá trị tại <code>mat[row][column]</code> thành <code>val</code>.</li>
	<li><code>int get(int row, char column)</code> Trả về giá trị tại <code>mat[row][column]</code>.</li>
	<li><code>int sum(int row, char column, List&lt;String&gt; numbers)</code> Đặt giá trị tại <code>mat[row][column]</code> bằng tổng các ô được biểu diễn bởi <code>numbers</code>, rồi trả về giá trị tại <code>mat[row][column]</code>. Công thức tính tổng này <strong>phải được duy trì</strong> cho đến khi ô bị ghi đè bằng một giá trị khác hoặc một công thức tính tổng khác. <code>numbers[i]</code> có thể có định dạng:
	<ul>
		<li><code>&quot;ColRow&quot;</code> biểu diễn một ô.
		<ul>
			<li>Ví dụ, <code>&quot;F7&quot;</code> represents the cell <code>mat[7][&#39;F&#39;]</code>.</li>
		</ul>
		</li>
		<li><code>&quot;ColRow1:ColRow2&quot;</code> biểu diễn một vùng ô. Vùng luôn là hình chữ nhật, trong đó <code>&quot;ColRow1&quot;</code> là vị trí ô trên cùng bên trái và <code>&quot;ColRow2&quot;</code> là vị trí ô dưới cùng bên phải.
		<ul>
			<li>Ví dụ, <code>&quot;B3:F7&quot;</code> represents the cells <code>mat[i][j]</code> for <code>3 &lt;= i &lt;= 7</code> and <code>&#39;B&#39; &lt;= j &lt;= &#39;F&#39;</code>.</li>
		</ul>
		</li>
	</ul>
	</li>
</ul>

<p><strong>Lưu ý:</strong> Có thể giả định rằng sẽ không có tham chiếu công thức tính tổng vòng lặp.</p>

<ul>
	<li>Ví dụ, <code>mat[1][&#39;A&#39;] == sum(1, &quot;B&quot;)</code> and <code>mat[1][&#39;B&#39;] == sum(1, &quot;A&quot;)</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Excel&quot;, &quot;set&quot;, &quot;sum&quot;, &quot;set&quot;, &quot;get&quot;]
[[3, &quot;C&quot;], [1, &quot;A&quot;, 2], [3, &quot;C&quot;, [&quot;A1&quot;, &quot;A1:B2&quot;]], [2, &quot;B&quot;, 2], [3, &quot;C&quot;]]
<strong>Đầu ra</strong>
[null, null, 4, null, 6]

<strong>Giải thích</strong>
Excel excel = new Excel(3, &quot;C&quot;);
 // construct a 3*3 2D array with all zero.
 //   A B C
 // 1 0 0 0
 // 2 0 0 0
 // 3 0 0 0
excel.set(1, &quot;A&quot;, 2);
 // set mat[1][&quot;A&quot;] to be 2.
 //   A B C
 // 1 2 0 0
 // 2 0 0 0
 // 3 0 0 0
excel.sum(3, &quot;C&quot;, [&quot;A1&quot;, &quot;A1:B2&quot;]); // return 4
 // set mat[3][&quot;C&quot;] to be the sum of value at mat[1][&quot;A&quot;] and the values sum of the rectangle range whose top-left cell is mat[1][&quot;A&quot;] and bottom-right cell is mat[2][&quot;B&quot;].
 //   A B C
 // 1 2 0 0
 // 2 0 0 0
 // 3 0 0 4
excel.set(2, &quot;B&quot;, 2);
 // set mat[2][&quot;B&quot;] to be 2. Note mat[3][&quot;C&quot;] should also be changed.
 //   A B C
 // 1 2 0 0
 // 2 0 2 0
 // 3 0 0 6
excel.get(3, &quot;C&quot;); // return 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= height &lt;= 26</code></li>
	<li><code>&#39;A&#39; &lt;= width &lt;= &#39;Z&#39;</code></li>
	<li><code>1 &lt;= row &lt;= height</code></li>
	<li><code>&#39;A&#39; &lt;= column &lt;= width</code></li>
	<li><code>-100 &lt;= val &lt;= 100</code></li>
	<li><code>1 &lt;= numbers.length &lt;= 5</code></li>
	<li><code>numbers[i]</code> có định dạng <code>&quot;ColRow&quot;</code> hoặc <code>&quot;ColRow1:ColRow2&quot;</code>.</li>
	<li>Sẽ có tối đa <code>100</code> lần gọi các hàm <code>set</code>, <code>get</code> và <code>sum</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bảng tính nhỏ, nhưng công thức `sum` phải luôn được cập nhật: khi một ô được tham chiếu thay đổi thì tổng cũng phải thay đổi. Giá trị đã cache mà không được tính lại sẽ trở nên lỗi thời.
>
> Lưu một giá trị cố định hoặc một công thức trong mỗi ô; `get`/`sum` tính đệ quy, còn `set` ghi đè công thức. Đề bài không cho phép tham chiếu vòng. Các tab lời giải hiện vẫn để trống.

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
