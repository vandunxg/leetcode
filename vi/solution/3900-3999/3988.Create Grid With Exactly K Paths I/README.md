---
comments: true
difficulty: Medium
rating: 2054
source: Weekly Contest 510 Q3
tags:
    - Array
    - Math
    - Combinatorics
    - Matrix
---

<!-- problem:start -->

# [3988. Create Grid With Exactly K Paths I](https://leetcode.com/problems/create-grid-with-exactly-k-paths-i)

[中文文档](/solution/3900-3999/3988.Create%20Grid%20With%20Exactly%20K%20Paths%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>m</code>, <code>n</code> và <code>k</code>.</p>

<p>Hãy xây dựng <strong>bất kỳ</strong> lưới <code>m x n</code> nào chỉ gồm các ký tự <code>&#39;.&#39;</code> và <code>&#39;#&#39;</code>, trong đó:</p>

<ul>
	<li><code>&#39;.&#39;</code> biểu diễn một ô tự do.</li>
	<li><code>&#39;#&#39;</code> biểu diễn một ô chướng ngại vật.</li>
</ul>

<p>Một <strong>đường đi hợp lệ</strong> là một dãy các ô tự do:</p>

<ul>
	<li>Bắt đầu tại ô trên cùng bên trái <code>(0, 0)</code>.</li>
	<li>Kết thúc tại ô dưới cùng bên phải <code>(m - 1, n - 1)</code>.</li>
	<li>Chỉ di chuyển:
	<ul>
		<li>Sang phải, từ <code>(i, j)</code> đến <code>(i, j + 1)</code>, hoặc</li>
		<li>Xuống dưới, từ <code>(i, j)</code> đến <code>(i + 1, j)</code>.</li>
	</ul>
	</li>
</ul>

<p>Trả về một lưới bất kỳ sao cho có <strong>chính xác</strong> <code>k</code> <strong>đường đi hợp lệ</strong> từ ô trên cùng bên trái đến ô dưới cùng bên phải. Nếu không tồn tại lưới như vậy, trả về một mảng rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 3, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;...&quot;,&quot;#..&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3988.Create%20Grid%20With%20Exactly%20K%20Paths%20I/images/screenshot-2026-05-27-at-113554am.png" style="width: 200px; height: 90px;" /></p>

<p>Có chính xác <code>k = 2</code> đường đi hợp lệ từ <code>(0, 0)</code> đến <code>(1, 2)</code>:</p>

<ul>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (0, 2) &rarr; (1, 2)</code></li>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (1, 2)</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 3, n = 3, k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;..#&quot;,&quot;...&quot;,&quot;#..&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3988.Create%20Grid%20With%20Exactly%20K%20Paths%20I/images/screenshot-2026-05-27-at-113452am.png" style="width: 250px; height: 178px;" /></p>

<p>Có chính xác <code>k = 4</code> đường đi hợp lệ từ <code>(0, 0)</code> đến <code>(2, 2)</code>:</p>

<ul>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (1, 2) &rarr; (2, 2)</code></li>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (2, 1) &rarr; (2, 2)</code></li>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (1, 2) &rarr; (2, 2)</code></li>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (2, 1) &rarr; (2, 2)</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 1, n = 4, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong>​</p>

<p>Không tồn tại lưới nào có chính xác <code>k = 2</code> đường đi hợp lệ đối với lưới <code>1 x 4</code>, nên đáp án là một mảng rỗng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 10</code></li>
	<li><code>1 &lt;= k &lt;= 4</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Số đường đi chỉ sang phải/xuống dưới là quy hoạch động trên lưới quen thuộc khi có chướng ngại vật. Để đạt chính xác $k$, các chướng ngại vật có thể tạo thành một phễu mà tại các nút giao, số đường đi được cộng theo các giá trị dạng Fibonacci hoặc nhị thức.
>
> Số lớn nhất có thể biểu diễn là hệ số nhị thức của lưới không có chướng ngại vật; nếu $k$ lớn hơn giá trị này thì không thể thực hiện. Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng ở việc phân rã $k$ bằng các bức tường.

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
