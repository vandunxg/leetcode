---
comments: true
difficulty: Medium
rating: 1573
source: Biweekly Contest 146 Q2
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3393. Count Paths With the Given XOR Value](https://leetcode.com/problems/count-paths-with-the-given-xor-value)

[中文文档](/solution/3300-3399/3393.Count%20Paths%20With%20the%20Given%20XOR%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>grid</code> có kích thước <code>m x n</code>. Đồng thời, bạn được cho một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là tính số đường đi từ ô góc trên bên trái <code>(0, 0)</code> đến ô góc dưới bên phải <code>(m - 1, n - 1)</code> thỏa mãn các <strong>ràng buộc</strong> sau:</p>

<ul>
	<li>Bạn chỉ có thể di chuyển sang phải hoặc xuống dưới. Cụ thể, từ ô <code>(i, j)</code>, bạn có thể di chuyển đến ô <code>(i, j + 1)</code> hoặc <code>(i + 1, j)</code> nếu ô đích <em>tồn tại</em>.</li>
	<li>Phép <code>XOR</code> của tất cả các số trên đường đi phải <strong>bằng</strong> <code>k</code>.</li>
</ul>

<p>Trả về tổng số đường đi như vậy.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[2, 1, 5], [7, 10, 0], [12, 6, 4]], k = 11</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>3 đường đi là:</p>

<ul>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (2, 0) &rarr; (2, 1) &rarr; (2, 2)</code></li>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (1, 2) &rarr; (2, 2)</code></li>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (2, 1) &rarr; (2, 2)</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1, 3, 3, 3], [0, 3, 3, 2], [3, 0, 1, 1]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>5 đường đi là:</p>

<ul>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (2, 0) &rarr; (2, 1) &rarr; (2, 2) &rarr; (2, 3)</code></li>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (2, 1) &rarr; (2, 2) &rarr; (2, 3)</code></li>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (1, 1) &rarr; (1, 2) &rarr; (1, 3) &rarr; (2, 3)</code></li>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1) &rarr; (1, 2) &rarr; (2, 2) &rarr; (2, 3)</code></li>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (0, 2) &rarr; (1, 2) &rarr; (2, 2) &rarr; (2, 3)</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1, 1, 1, 2], [3, 0, 3, 2], [3, 0, 2, 2]], k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m == grid.length &lt;= 300</code></li>
	<li><code>1 &lt;= n == grid[r].length &lt;= 300</code></li>
	<li><code>0 &lt;= grid[r][c] &lt; 16</code></li>
	<li><code>0 &lt;= k &lt; 16</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta đi từ ô góc trên bên trái đến ô góc dưới bên phải (chỉ sang phải hoặc xuống dưới) với XOR của đường đi bằng $k$. Vì lưới có kích thước tối đa $300 \times 300$ và các giá trị nhỏ hơn $16$, ta có thể dùng $f[i][j][x]$.
>
> Miền giá trị của XOR có kích thước $16$. Khi chuyển vào $(i,j)$, ta lấy XOR của $\textit{grid}[i][j]$ với các đường đi từ phía trên và bên trái.
>
> Đáp án là $f[m-1][n-1][k]$ lấy modulo $10^9+7$.

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
