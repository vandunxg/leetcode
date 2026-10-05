---
comments: true
difficulty: Medium
rating: 1770
source: Biweekly Contest 179 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3882. Minimum XOR Path in a Grid](https://leetcode.com/problems/minimum-xor-path-in-a-grid)

[中文文档](/solution/3800-3899/3882.Minimum%20XOR%20Path%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>grid</code> có kích thước <code>m * n</code>.</p>

<p>Bạn bắt đầu tại ô <strong>góc trên bên trái</strong> <code>(0, 0)</code> và muốn đi đến ô <strong>góc dưới bên phải</strong> <code>(m - 1, n - 1)</code>.</p>

<p>Ở mỗi bước, bạn <strong>có thể</strong> di chuyển sang <strong>phải hoặc xuống</strong>.</p>

<p><strong>Chi phí</strong> của một đường đi được định nghĩa là <strong>phép XOR bitwise</strong> của tất cả các giá trị trong những ô trên đường đi đó, <strong>bao gồm</strong> ô bắt đầu và ô kết thúc.</p>

<p>Trả về giá trị XOR <strong>nhỏ nhất</strong> có thể có trong tất cả các đường đi hợp lệ từ <code>(0, 0)</code> đến <code>(m - 1, n - 1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có hai đường đi hợp lệ:</p>

<ul>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1)</code> với XOR: <code>1 XOR 2 XOR 4 = 7</code></li>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (1, 1)</code> với XOR: <code>1 XOR 3 XOR 4 = 6</code></li>
</ul>

<p>Giá trị XOR nhỏ nhất trong các đường đi hợp lệ là 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[6,7],[5,8]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có hai đường đi hợp lệ:</p>

<ul>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (1, 1)</code> với XOR: <code>6 XOR 7 XOR 8 = 9</code></li>
	<li><code>(0, 0) &rarr; (1, 0) &rarr; (1, 1)</code> với XOR: <code>6 XOR 5 XOR 8 = 11</code></li>
</ul>

<p>Giá trị XOR nhỏ nhất trong các đường đi hợp lệ là 9.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[2,7,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một đường đi hợp lệ:</p>

<ul>
	<li><code>(0, 0) &rarr; (0, 1) &rarr; (0, 2)</code> với XOR: <code>2 XOR 7 XOR 5 = 0</code></li>
</ul>

<p>Giá trị XOR của đường đi này là 0, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m == grid.length &lt;= 1000</code></li>
	<li><code>1 &lt;= n == grid[i].length &lt;= 1000</code></li>
	<li><code>m * n &lt;= 1000</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 1023​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ được di chuyển sang phải hoặc xuống và cần tối thiểu hóa XOR của đường đi. Có nhiều nhất $1000$ ô, các giá trị đều nhỏ hơn $2^{10}$.
>
> Cách tiếp cận đường đi ngắn nhất thông thường không phù hợp: chi phí tiếp theo phụ thuộc vào XOR hiện tại, nên tính tối ưu con không được đảm bảo.
>
> Một trạng thái là $(\textit{cell},\textit{xor})$. Miền giá trị XOR có $1024$ giá trị, tương ứng khoảng $10^6$ trạng thái, đủ để dùng BFS hoặc DP tìm giá trị nhỏ nhất tại ô dưới bên phải.
>
> Các phép chuyển trạng thái chỉ đi sang phải hoặc xuống.

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
