---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - Math
    - Combinatorics
    - Matrix
---

<!-- problem:start -->

# [3990. Create Grid With Exactly K Paths II 🔒](https://leetcode.com/problems/create-grid-with-exactly-k-paths-ii)

[中文文档](/solution/3900-3999/3990.Create%20Grid%20With%20Exactly%20K%20Paths%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>k</code>.</p>

<p>Hãy xây dựng <strong>bất kỳ</strong> lưới nào chỉ gồm các ký tự <code>&#39;.&#39;</code> và <code>&#39;#&#39;</code>, trong đó:</p>

<ul>
	<li><code>&#39;.&#39;</code> biểu diễn một ô tự do.</li>
	<li><code>&#39;#&#39;</code> biểu diễn một ô chướng ngại vật.</li>
</ul>

<p>Lưới phải có <strong>không quá</strong> 25 hàng và <strong>không quá</strong> 25 cột.</p>

<p>Một <strong>đường đi hợp lệ</strong> là một dãy các ô tự do:</p>

<ul>
	<li>Bắt đầu tại ô trên cùng bên trái <code>(0, 0)</code>.</li>
	<li>Kết thúc tại ô dưới cùng bên phải <code>(m - 1, n - 1)</code>, trong đó <code>m</code> và <code>n</code> là kích thước của lưới được xây dựng.</li>
	<li>Chỉ di chuyển:
	<ul>
		<li>Sang phải, từ <code>(i, j)</code> đến <code>(i, j + 1)</code>, hoặc</li>
		<li>Xuống dưới, từ <code>(i, j)</code> đến <code>(i + 1, j)</code>.</li>
	</ul>
	</li>
</ul>

<p>Trả về một lưới bất kỳ sao cho có <strong>chính xác <code>k</code> đường đi hợp lệ</strong> từ ô trên cùng bên trái đến ô dưới cùng bên phải. Nếu không tồn tại lưới như vậy, trả về một mảng rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;..#&quot;,&quot;#..&quot;,&quot;#..&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3990.Create%20Grid%20With%20Exactly%20K%20Paths%20II/images/screenshot-2026-05-31-at-82224pm.png" style="width: 200px; height: 135px;" /></p>

<p>Lưới chứa chính xác 2 đường đi hợp lệ từ <code>(0, 0)</code> đến <code>(2, 2)</code>:</p>

<ul>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (1, 2) &rarr; (2, 2)</code></li>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (2, 1) &rarr; (2, 2)</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;...&quot;,&quot;#..&quot;,&quot;#..&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong>​​​​​​​</strong><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3990.Create%20Grid%20With%20Exactly%20K%20Paths%20II/images/screenshot-2026-05-31-at-82251pm.png" style="width: 200px; height: 128px;" /></p>

<p>Lưới chứa chính xác 3 đường đi hợp lệ từ <code>(0, 0)</code> đến <code>(2, 2)</code>:</p>

<ul>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (0, 2) &rarr; (1, 2) &rarr; (2, 2)</code></li>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (1, 2) &rarr; (2, 2)</code></li>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (2, 1) &rarr; (2, 2)</code></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong>​​​​​​​</p>

<ul>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mục tiêu giống phần I, nhưng kích thước lưới được tự chọn (không quá $25\times 25$). Một trục chính cùng các đường vòng có độ dài $t$ có thể biểu diễn $k$ thành tổng của các khối đường đi dạng nhị phân hoặc Fibonacci.
>
> $k\le 1000$ vừa trong giới hạn $25$ ô. Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng ở việc phân bổ $k$ vào các kích thước đã chọn.

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
