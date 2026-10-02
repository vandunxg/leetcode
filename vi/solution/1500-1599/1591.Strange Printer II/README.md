---
comments: true
difficulty: Hard
rating: 2290
source: Biweekly Contest 35 Q4
tags:
    - Graph
    - Topological Sort
    - Array
    - Directed Acyclic Graph
    - Matrix
---

<!-- problem:start -->

# [1591. Strange Printer II](https://leetcode.com/problems/strange-printer-ii)

[中文文档](/solution/1500-1599/1591.Strange%20Printer%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có một máy in đặc biệt với hai yêu cầu sau:</p>

<ul>
	<li>Mỗi lượt, máy in một hình chữ nhật đặc kín bằng một màu trên lưới. Hình chữ nhật này sẽ che các màu hiện có trong vùng đó.</li>
	<li>Sau khi đã dùng một màu cho thao tác trên, <strong>không được dùng lại màu đó</strong>.</li>
</ul>

<p>Cho ma trận <code>m x n</code> <code>targetGrid</code>, trong đó <code>targetGrid[row][col]</code> là màu tại vị trí <code>(row, col)</code> của lưới.</p>

<p>Trả về <code>true</code><em> nếu có thể in ma trận </em><code>targetGrid</code><em>,</em><em> ngược lại trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1591.Strange%20Printer%20II/images/print1.jpg" style="width: 600px; height: 175px;" />
<pre>
<strong>Input:</strong> targetGrid = [[1,1,1,1],[1,2,2,1],[1,2,2,1],[1,1,1,1]]
<strong>Output:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1591.Strange%20Printer%20II/images/print2.jpg" style="width: 600px; height: 367px;" />
<pre>
<strong>Input:</strong> targetGrid = [[1,1,1,1],[1,1,3,3],[1,1,3,4],[5,5,1,4]]
<strong>Output:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> targetGrid = [[1,2,1],[2,1,2],[1,2,1]]
<strong>Output:</strong> false
<strong>Giải thích:</strong> Không thể tạo targetGrid vì không được in cùng một màu ở các lượt khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == targetGrid.length</code></li>
	<li><code>n == targetGrid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 60</code></li>
	<li><code>1 &lt;= targetGrid[row][col] &lt;= 60</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần in đặt một màu mới thành hình chữ nhật và màu đó không thể dùng lại. Màu đó phải phủ bounding box của nó, còn các màu in sau sẽ ghi đè các ô bên trong bounding box.
>
> Với mỗi màu, lấy hàng và cột nhỏ nhất/lớn nhất. Mọi màu khác $c'$ nằm trong hình chữ nhật đó phải được in sau, nên ta thêm cạnh $c\to c'$. Có thể in được khi và chỉ khi đồ thị ràng buộc không có chu trình, điều này được quyết định bằng topological sort.

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
