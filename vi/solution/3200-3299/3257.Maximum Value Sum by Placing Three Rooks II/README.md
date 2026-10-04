---
comments: true
difficulty: Hard
rating: 2553
source: Biweekly Contest 137 Q4
tags:
    - Array
    - Dynamic Programming
    - Enumeration
    - Matrix
---

<!-- problem:start -->

# [3257. Maximum Value Sum by Placing Three Rooks II](https://leetcode.com/problems/maximum-value-sum-by-placing-three-rooks-ii)

[中文文档](/solution/3200-3299/3257.Maximum%20Value%20Sum%20by%20Placing%20Three%20Rooks%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng 2D kích thước <code>m x n</code> là <code>board</code>, biểu diễn một bàn cờ, trong đó <code>board[i][j]</code> là <strong>giá trị</strong> của ô <code>(i, j)</code>.</p>

<p>Các quân xe trên <strong>cùng</strong> hàng hoặc cột sẽ <strong>tấn công</strong> lẫn nhau. Hãy đặt <em>ba</em> quân xe lên bàn cờ sao cho các quân xe <strong>không</strong> <strong>tấn công</strong> lẫn nhau.</p>

<p>Trả về tổng <strong>lớn nhất</strong> của các <strong>giá trị</strong> tại những ô đặt quân xe.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">board = </span>[[-3,1,1,1],[-3,1,-3,1],[-3,2,1,1]]</p>

<p><strong>Đầu ra:</strong> 4</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3257.Maximum%20Value%20Sum%20by%20Placing%20Three%20Rooks%20II/images/rooks2.png" style="width: 294px; height: 450px;" /></p>

<p>Ta có thể đặt các quân xe vào các ô <code>(0, 2)</code>, <code>(1, 3)</code> và <code>(2, 1)</code>, khi đó tổng là <code>1 + 1 + 2 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">board = [[1,2,3],[4,5,6],[7,8,9]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể đặt các quân xe vào các ô <code>(0, 0)</code>, <code>(1, 1)</code> và <code>(2, 2)</code>, khi đó tổng là <code>1 + 5 + 9 = 15</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">board = [[1,1,1],[1,1,1],[1,1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể đặt các quân xe vào các ô <code>(0, 2)</code>, <code>(1, 1)</code> và <code>(2, 0)</code>, khi đó tổng là <code>1 + 1 + 1 = 3</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= m == board.length &lt;= 500</code></li>
	<li><code>3 &lt;= n == board[i].length &lt;= 500</code></li>
	<li><code>-10<sup>9</sup> &lt;= board[i][j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tương tự bài I, nhưng độ dài cạnh là $500$, nên ngay cả việc chọn một vài ứng viên mỗi hàng cũng khiến việc liệt kê bộ ba hàng trở nên nặng. Ta tiếp tục chỉ giữ các ô có giá trị lớn nhất ở mỗi cột, hoặc chỉ liệt kê các ô lớn nhất trên toàn bảng.
>
> Các quân xe vẫn cần nằm trên những hàng và cột khác nhau; các ứng viên được lấy từ một số ít ô có giá trị lớn nhất của mỗi hàng hoặc cột. Hiện chưa có phần cài đặt trong cây; ý tưởng là thu nhỏ tập ứng viên, rồi kiểm tra xung đột.

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
