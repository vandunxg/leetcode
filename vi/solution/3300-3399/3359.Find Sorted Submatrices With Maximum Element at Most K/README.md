---
comments: true
difficulty: Hard
tags:
    - Stack
    - Array
    - Matrix
    - Monotonic Stack
---

<!-- problem:start -->

# [3359. Find Sorted Submatrices With Maximum Element at Most K 🔒](https://leetcode.com/problems/find-sorted-submatrices-with-maximum-element-at-most-k)

[中文文档](/solution/3300-3399/3359.Find%20Sorted%20Submatrices%20With%20Maximum%20Element%20at%20Most%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận 2D <code>grid</code> có kích thước <code>m x n</code>. Đồng thời, bạn được cho một số nguyên <strong>không âm</strong> <code>k</code>.</p>

<p>Hãy trả về số lượng <strong>ma trận con</strong> của <code>grid</code> thỏa mãn các điều kiện sau:</p>

<ul>
    <li>Phần tử lớn nhất trong ma trận con <strong>nhỏ hơn hoặc bằng</strong> <code>k</code>.</li>
    <li>Mỗi hàng trong ma trận con được sắp xếp theo thứ tự <strong>không tăng</strong>.</li>
</ul>

<p>Ma trận con <code>(x1, y1, x2, y2)</code> là ma trận được tạo thành bằng cách chọn tất cả các ô <code>grid[x][y]</code> với <code>x1 &lt;= x &lt;= x2</code> và <code>y1 &lt;= y &lt;= y2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[4,3,2,1],[8,7,6,1]], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3359.Find%20Sorted%20Submatrices%20With%20Maximum%20Element%20at%20Most%20K/images/mine.png" style="width: 360px; height: 200px;" /></strong></p>

<p>Có 8 ma trận con:</p>

<ul>
    <li><code>[[1]]</code></li>
    <li><code>[[1]]</code></li>
    <li><code>[[2,1]]</code></li>
    <li><code>[[3,2,1]]</code></li>
    <li><code>[[1],[1]]</code></li>
    <li><code>[[2]]</code></li>
    <li><code>[[3]]</code></li>
    <li><code>[[3,2]]</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,1,1],[1,1,1],[1,1,1]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">36</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 36 ma trận con trong grid. Mọi ma trận con đều có phần tử lớn nhất bằng 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == grid.length &lt;= 10<sup>3</sup></code></li>
    <li><code>1 &lt;= n == grid[i].length &lt;= 10<sup>3</sup></code></li>
    <li><code>1 &lt;= grid[i][j] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
​​​​​​

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta đếm các ma trận con có các cột không tăng từ trên xuống dưới và các phần tử không vượt quá $k$. Với $m,n \le 10^3$, cần xử lý mỗi góc dưới bên phải trong thời gian gần tuyến tính.
>
> Loại bỏ các ô lớn hơn $k$, sau đó lưu chiều cao không tăng hướng lên trong mỗi cột. Trên mỗi hàng, các chiều cao này tạo thành một histogram.
>
> Một stack đơn điệu đếm các hình chữ nhật có biên phải là cột hiện tại; tổng trên tất cả các ô là đáp án.

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
