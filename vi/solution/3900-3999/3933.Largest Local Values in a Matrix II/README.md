---
comments: true
difficulty: Medium
rating: 2052
source: Weekly Contest 502 Q3
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3933. Largest Local Values in a Matrix II](https://leetcode.com/problems/largest-local-values-in-a-matrix-ii)

[中文文档](/solution/3900-3999/3933.Largest%20Local%20Values%20in%20a%20Matrix%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên <code>n x m</code> <code>matrix</code> chứa các số nguyên không âm.</p>

<p>Một ô <strong>khác không</strong> <code>(row, col)</code> kiểm tra các ô lân cận như sau:</p>

<ul>
	<li>Gọi <code>x = matrix[row][col]</code>.</li>
	<li>Xét mọi ô nằm trong phạm vi <code>x</code> hàng và <code>x</code> cột tính từ <code>(row, col)</code>.</li>
	<li>Bỏ qua các ô nằm ngoài ma trận.</li>
	<li>Bỏ qua các ô mà cả khoảng cách theo hàng và khoảng cách theo cột đều đúng bằng <code>x</code>.</li>
</ul>

<p>Ô <code>(row, col)</code> là một <strong>cực đại cục bộ</strong> nếu nó <strong>khác không</strong> và không có ô nào được xét có giá trị <strong>lớn hơn</strong> <code>x</code>.</p>

<p>Trả về một số nguyên biểu thị số lượng <strong>cực đại cục bộ</strong> trong <code>matrix</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,2,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3933.Largest%20Local%20Values%20in%20a%20Matrix%20II/images/chatgpt-image-may-14-2026-01_53_19-am.png" style="width: 300px; height: 300px;" /></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với ô khác không <code>(3, 3)</code>, <code>x = matrix[3][3] = 2</code>.</li>
	<li>Các ô được tô sáng là những ô được xét, nằm trong phạm vi <code>x</code> hàng và <code>x</code> cột tính từ <code>(3, 3)</code>.</li>
	<li>Bốn ô có cả khoảng cách theo hàng và cột bằng <code>x = 2</code> được bỏ qua.</li>
	<li>Không có ô nào được xét có giá trị lớn hơn 2, nên <code>(3, 3)</code> là một cực đại cục bộ.</li>
	<li>Không còn ô khác không nào, nên đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[1,2],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ ô có giá trị 4 là một cực đại cục bộ. Mọi ô khác không đều xét một ô có giá trị lớn hơn.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[1,0,1],[0,1,0],[1,0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với một ô có giá trị 1, các ô được xét là chính nó và các ô kề theo 4 hướng nằm trong ma trận.</li>
	<li>Cả năm ô có giá trị 1 chỉ xét các ô có giá trị 0 hoặc 1, nên cả năm đều là cực đại cục bộ.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[1,1],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi ô đều có cùng giá trị. Vì vậy, không ô nào xét một ô khác có giá trị lớn hơn, nên cả 4 ô đều là cực đại cục bộ.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == matrix.length &lt;= 200</code></li>
	<li><code>1 &lt;= m == matrix[i].length &lt;= 200</code></li>
	<li><code>0 &lt;= matrix[i][j] &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ô khác không kiểm tra một vùng lân cận có bán kính $x=\textit{matrix}[r][c]$ (bỏ qua bốn ô ở khoảng cách Chebyshev đúng bằng $x$). Cách duyệt ngây thơ có độ phức tạp $O(nm\cdot x^2)$ và vẫn sát giới hạn khi $x\le 200$. Điều kiện cực đại cục bộ có nghĩa là không có giá trị nào lớn hơn nằm trong vùng lân cận đó.
>
> Duyệt các giá trị từ lớn đến nhỏ cho phép các phần tử lớn hơn loại các ứng viên nhỏ hơn. Một cách khác là dùng sparse table 2D để lấy giá trị lớn nhất của hình chữ nhật trong $O(1)$ sau khi loại bốn góc bị bỏ qua.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần hướng dẫn dừng ở việc truy vấn giá trị lớn nhất trong vùng lân cận.

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
