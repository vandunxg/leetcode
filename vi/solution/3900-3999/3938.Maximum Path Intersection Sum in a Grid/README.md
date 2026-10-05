---
comments: true
difficulty: Medium
rating: 2251
source: Biweekly Contest 183 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3938. Maximum Path Intersection Sum in a Grid](https://leetcode.com/problems/maximum-path-intersection-sum-in-a-grid)

[中文文档](/solution/3900-3999/3938.Maximum%20Path%20Intersection%20Sum%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p data-end="139" data-start="64">Bạn được cho một ma trận số nguyên <code>m x n</code> <code>grid</code>.</p>

<p>Có hai người chơi di chuyển qua ma trận:</p>

<ul>
	<li>Người chơi 1 bắt đầu tại ô góc trên bên trái <code>(0, 0)</code> và chỉ có thể di chuyển sang phải hoặc xuống dưới. Điểm đến là ô góc dưới bên phải <code>(m - 1, n - 1)</code>.</li>
	<li>Người chơi 2 bắt đầu tại ô góc dưới bên trái <code>(m - 1, 0)</code> và chỉ có thể di chuyển sang phải hoặc lên trên. Điểm đến là ô góc trên bên phải <code>(0, n - 1)</code>.</li>
</ul>

<p>Mỗi người chơi phải chọn một đường đi hợp lệ từ ô bắt đầu tương ứng đến điểm đến.</p>

<p>Một ô được gọi là <strong>chung</strong> nếu nó thuộc về <strong>cả hai</strong> đường đi đã chọn.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng</strong> giá trị lớn nhất có thể của tất cả các ô <strong>chung</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3938.Maximum%20Path%20Intersection%20Sum%20in%20a%20Grid/images/image.png" style="width: 200px; height: 251px;" />​​​​​​​​​​​​​​​​​​​​​
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2,0,-3],[1,-2,1,0],[-4,2,-1,3],[3,-3,3,-2],[-1,-5,0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>
Sơ đồ minh họa một cách chọn đường đi tối ưu.

<ul>
	<li>Người chơi 1 đi theo đường màu đỏ/tím từ ô góc trên bên trái đến ô góc dưới bên phải:
	<ul>
		<li><code>(0, 0) &rarr; (1, 0) &rarr; (2, 0) &rarr; (2, 1) &rarr; (2, 2) &rarr; (2, 3) &rarr; (3, 3) &rarr; (4, 3)</code></li>
	</ul>
	</li>
	<li>Người chơi 2 đi theo đường màu xanh/tím từ ô góc dưới bên trái đến ô góc trên bên phải:
	<ul>
		<li><code>(4, 0) &rarr; (4, 1) &rarr; (3, 1) &rarr; (2, 1) &rarr; (2, 2) &rarr; (2, 3) &rarr; (1, 3) &rarr; (0, 3)</code></li>
	</ul>
	</li>
	<li>Các ô chung là <code>(2, 1)</code>, <code>(2, 2)</code> và <code>(2, 3)</code>.</li>
	<li>Tổng là <code>2 + (-1) + 3 = 4</code>, đây là tổng lớn nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3938.Maximum%20Path%20Intersection%20Sum%20in%20a%20Grid/images/chatgpt-image-may-19-2026-01_39_39-pm.png" style="width: 200px; height: 200px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[4,-2,-3],[-1,-3,-1],[-4,2,-1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cặp đường đi tối ưu được minh họa trong sơ đồ.</p>

<ul>
	<li>Người chơi 1 đi theo đường màu đỏ/tím:
	<ul>
		<li><code>(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (1, 2) &rarr; (2, 2)</code></li>
	</ul>
	</li>
	<li>Người chơi 2 đi theo đường màu xanh/tím:
	<ul>
		<li><code>(2, 0) &rarr; (1, 0) &rarr; (0, 0) &rarr; (0, 1) &rarr; (0, 2)</code></li>
	</ul>
	</li>
	<li>Các ô chung là <code>(0, 0)</code> và <code>(1, 0)</code>.</li>
	<li>Tổng là <code>4 + (-1) = 3</code>, đây là tổng lớn nhất có thể.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 1000</code></li>
	<li><code>4 &lt;= m * n &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>-100 &lt;= grid[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các ô chung của hai đường đi phải đồng thời nằm trên một đường đi từ góc trên bên trái đến góc dưới bên phải và một đường đi từ góc dưới bên trái đến góc trên bên phải. Với $m,n$ lên đến $10^3$, việc liệt kê các cặp đường đi là bất khả thi.
>
> Phần giao nhau về bản chất là một hành lang đi qua một số cột. Với mỗi dải ứng viên, ta cộng các phần tiếp cận bắt buộc bên ngoài dải vào các giá trị trong dải; bốn phép quy hoạch động từ bốn góc sẽ tính trước các phần tiếp cận này.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần hướng dẫn dừng ở “quy hoạch động đường đi từ bốn góc cộng với một dải giao nhau”.

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
