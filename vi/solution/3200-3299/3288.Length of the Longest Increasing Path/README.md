---
comments: true
difficulty: Hard
rating: 2449
source: Biweekly Contest 139 Q4
tags:
    - Array
    - Binary Search
    - Sorting
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [3288. Length of the Longest Increasing Path](https://leetcode.com/problems/length-of-the-longest-increasing-path)

[中文文档](/solution/3200-3299/3288.Length%20of%20the%20Longest%20Increasing%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên hai chiều <code>coordinates</code> có độ dài <code>n</code> và một số nguyên <code>k</code>, trong đó <code>0 &lt;= k &lt; n</code>.</p>

<p><code>coordinates[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn điểm <code>(x<sub>i</sub>, y<sub>i</sub>)</code> trên mặt phẳng hai chiều.</p>

<p>Một <strong>đường đi tăng</strong> có độ dài <code>m</code> được định nghĩa là một danh sách các điểm <code>(x<sub>1</sub>, y<sub>1</sub>)</code>, <code>(x<sub>2</sub>, y<sub>2</sub>)</code>, <code>(x<sub>3</sub>, y<sub>3</sub>)</code>, ..., <code>(x<sub>m</sub>, y<sub>m</sub>)</code> sao cho:</p>

<ul>
	<li><code>x<sub>i</sub> &lt; x<sub>i + 1</sub></code> và <code>y<sub>i</sub> &lt; y<sub>i + 1</sub></code> với mọi <code>i</code> thỏa mãn <code>1 &lt;= i &lt; m</code>.</li>
	<li><code>(x<sub>i</sub>, y<sub>i</sub>)</code> thuộc mảng tọa độ đã cho với mọi <code>i</code> thỏa mãn <code>1 &lt;= i &lt;= m</code>.</li>
</ul>

<p>Trả về độ dài <strong>lớn nhất</strong> của một <strong>đường đi tăng</strong> chứa <code>coordinates[k]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coordinates = [[3,1],[2,2],[4,1],[0,0],[5,3]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>(0, 0)</code>, <code>(2, 2)</code>, <code>(5, 3)</code><!-- notionvc: 082cee9e-4ce5-4ede-a09d-57001a72141d --> là đường đi tăng dài nhất chứa <code>(2, 2)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coordinates = [[2,1],[7,0],[5,6]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>(2, 1)</code>, <code>(5, 6)</code> là đường đi tăng dài nhất chứa <code>(5, 6)</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == coordinates.length &lt;= 10<sup>5</sup></code></li>
	<li><code>coordinates[i].length == 2</code></li>
	<li><code>0 &lt;= coordinates[i][0], coordinates[i][1] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả phần tử trong <code>coordinates</code> đều <strong>khác nhau</strong>.<!-- notionvc: 6e412fc2-f9dd-4ba2-b796-5e802a2b305a --><!-- notionvc: c2cf5618-fe99-4909-9b4c-e6b068be22a6 --></li>
	<li><code>0 &lt;= k &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các điểm phải tăng ở cả $x$ và $y$, đồng thời đường đi phải chứa $\textit{coordinates}[k]$. Với $n\le 10^5$, không thể dùng LIS với độ phức tạp $O(n^2)$. Đáp án là LIS ở bên trái của $k$ cộng với LIS ở bên phải, rồi trừ đi một.
>
> Sắp xếp theo $x$ và duy trì LIS trên $y$ bằng Fenwick tree hoặc tìm kiếm nhị phân; phía bên trái chỉ sử dụng các điểm nhỏ hơn nghiêm ngặt $k$, phía bên phải chỉ sử dụng các điểm lớn hơn nghiêm ngặt. Hiện chưa có phần cài đặt trong cây; lập luận ở đây là tách LIS hai chiều thành hai phía.

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
