---
comments: true
difficulty: Medium
rating: 1883
source: Biweekly Contest 164 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3665. Twisted Mirror Path Count](https://leetcode.com/problems/twisted-mirror-path-count)

[中文文档](/solution/3600-3699/3665.Twisted%20Mirror%20Path%20Count/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một lưới nhị phân <code>m x n</code> <code>grid</code>, trong đó:</p>

<ul>
	<li><code>grid[i][j] == 0</code> biểu thị một ô trống, và</li>
	<li><code>grid[i][j] == 1</code> biểu thị một chiếc gương.</li>
</ul>

<p>Một robot bắt đầu ở góc trên bên trái của lưới <code>(0, 0)</code> và muốn đến góc dưới bên phải <code>(m - 1, n - 1)</code>. Robot chỉ có thể di chuyển <strong>sang phải</strong> hoặc <strong>đi xuống</strong>. Nếu robot cố di chuyển vào một ô gương, nó sẽ bị <strong>đổi hướng</strong> trước khi vào ô đó:</p>

<ul>
	<li>Nếu cố đi <strong>sang phải</strong> vào một chiếc gương, robot sẽ bị chuyển hướng <strong>xuống dưới</strong> và đi vào ô ngay bên dưới chiếc gương.</li>
	<li>Nếu cố đi <strong>xuống dưới</strong> vào một chiếc gương, robot sẽ bị chuyển hướng <strong>sang phải</strong> và đi vào ô ngay bên phải chiếc gương.</li>
</ul>

<p>Nếu việc đổi hướng khiến robot đi ra ngoài biên của <code>grid</code>, đường đi đó được xem là không hợp lệ và không được tính.</p>

<p>Trả về số đường đi hợp lệ khác nhau từ <code>(0, 0)</code> đến <code>(m - 1, n - 1)</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý</strong>: Nếu việc đổi hướng khiến robot đi vào một ô gương khác, robot sẽ ngay lập tức tiếp tục bị đổi hướng dựa trên hướng mà nó đã dùng để đi vào ô gương đó: nếu đi vào khi đang di chuyển sang phải, robot sẽ bị chuyển hướng xuống dưới; nếu đi vào khi đang di chuyển xuống dưới, robot sẽ bị chuyển hướng sang phải. Quá trình này tiếp tục cho đến khi robot đến ô cuối cùng, đi ra ngoài biên hoặc đi vào một ô không phải gương.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,1,0],[0,0,1],[1,0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Số thứ tự</th>
			<th align="left" style="border: 1px solid black;">Đường đi đầy đủ</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="left" style="border: 1px solid black;">(0, 0) &rarr; (0, 1) [M] &rarr; (1, 1) &rarr; (1, 2) [M] &rarr; (2, 2)</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="left" style="border: 1px solid black;">(0, 0) &rarr; (0, 1) [M] &rarr; (1, 1) &rarr; (2, 1) &rarr; (2, 2)</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="left" style="border: 1px solid black;">(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (1, 2) [M] &rarr; (2, 2)</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">4</td>
			<td align="left" style="border: 1px solid black;">(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (2, 1) &rarr; (2, 2)</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">5</td>
			<td align="left" style="border: 1px solid black;">(0, 0) &rarr; (1, 0) &rarr; (2, 0) [M] &rarr; (2, 1) &rarr; (2, 2)</td>
		</tr>
	</tbody>
</table>

<ul data-end="606" data-start="521">
	<li data-end="606" data-start="521">
	<p data-end="606" data-start="523"><code>[M]</code> cho biết robot đã cố đi vào một ô gương và bị đổi hướng thay vì đi vào ô đó.</p>
	</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,0],[0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Số thứ tự</th>
			<th align="left" style="border: 1px solid black;">Đường đi đầy đủ</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="left" style="border: 1px solid black;">(0, 0) &rarr; (0, 1) &rarr; (1, 1)</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="left" style="border: 1px solid black;">(0, 0) &rarr; (1, 0) &rarr; (1, 1)</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = </span>[[0,1,1],[1,1,0]]</p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Số thứ tự</th>
			<th align="left" style="border: 1px solid black;">Đường đi đầy đủ</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="left" style="border: 1px solid black;">(0, 0) &rarr; (0, 1) [M] &rarr; (1, 1) [M] &rarr; (1, 2)</td>
		</tr>
	</tbody>
</table>
<code>(0, 0) &rarr; (1, 0) [M] &rarr; (1, 1) [M] &rarr; (2, 1)</code> đi ra ngoài biên, nên không hợp lệ.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li data-end="41" data-start="21"><code data-end="39" data-start="21">m == grid.length</code></li>
	<li data-end="67" data-start="44"><code data-end="65" data-start="44">n == grid[i].length</code></li>
	<li data-end="91" data-start="70"><code data-end="89" data-start="70">2 &lt;= m, n &lt;= 500</code></li>
	<li data-end="129" data-start="94"><code data-end="106" data-start="94">grid[i][j]</code> là <code data-end="120" data-is-only-node="" data-start="117">0</code> hoặc <code data-end="127" data-start="124">1</code>.</li>
	<li data-end="169" data-start="132"><code data-end="167" data-start="132">grid[0][0] == grid[m - 1][n - 1] == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Gương bẻ hướng đi sang phải thành đi xuống và ngược lại. Vì vậy, các đường đi từ góc trên bên trái đến góc dưới bên phải cần lưu lại hướng di chuyển. Thực hiện phép lấy modulo $10^9+7$.
>
> Gọi $f[i][j][d]$ là số cách đi đến $(i,j)$ với hướng di chuyển cuối cùng là $d$. Ở ô trống, robot có thể tiếp tục đi sang phải hoặc đi xuống; ở ô gương, robot buộc phải đổi hướng.
>
> Cập nhật theo thứ tự tăng dần của $i+j$. Khởi tạo ô bắt đầu với một cách. Đáp án là tổng số cách theo các hướng tại ô đích.

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
