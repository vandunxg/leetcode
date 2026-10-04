---
comments: true
difficulty: Hard
rating: 2443
source: Biweekly Contest 148 Q4
tags:
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [3426. Manhattan Distances of All Arrangements of Pieces](https://leetcode.com/problems/manhattan-distances-of-all-arrangements-of-pieces)

[中文文档](/solution/3400-3499/3426.Manhattan%20Distances%20of%20All%20Arrangements%20of%20Pieces/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code><font face="monospace">m</font></code>, <code><font face="monospace">n</font></code> và <code>k</code>.</p>

<p>Có một lưới hình chữ nhật kích thước <code>m &times; n</code> chứa <code>k</code> quân cờ giống hệt nhau. Hãy trả về tổng khoảng cách Manhattan giữa mọi cặp quân cờ trong tất cả <strong>cách sắp xếp hợp lệ</strong> của các quân cờ.</p>

<p><strong>Cách sắp xếp hợp lệ</strong> là cách đặt tất cả <code>k</code> quân cờ lên lưới sao cho mỗi ô có <strong>nhiều nhất</strong> một quân cờ.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Khoảng cách Manhattan giữa hai ô <code>(x<sub>i</sub>, y<sub>i</sub>)</code> và <code>(x<sub>j</sub>, y<sub>j</sub>)</code> là <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 2, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cách sắp xếp quân cờ hợp lệ trên bàn cờ là:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3426.Manhattan%20Distances%20of%20All%20Arrangements%20of%20Pieces/images/4040example1.drawio" /><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3426.Manhattan%20Distances%20of%20All%20Arrangements%20of%20Pieces/images/untitled-diagramdrawio.png" style="width: 441px; height: 204px;" /></p>

<ul>
	<li>Trong 4 cách sắp xếp đầu tiên, khoảng cách Manhattan giữa hai quân cờ là 1.</li>
	<li>Trong 2 cách sắp xếp cuối cùng, khoảng cách Manhattan giữa hai quân cờ là 2.</li>
</ul>

<p>Do đó, tổng khoảng cách Manhattan trong tất cả các cách sắp xếp hợp lệ là <code>1 + 1 + 1 + 1 + 2 + 2 = 8</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 1, n = 4, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cách sắp xếp quân cờ hợp lệ trên bàn cờ là:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3426.Manhattan%20Distances%20of%20All%20Arrangements%20of%20Pieces/images/4040example2drawio.png" style="width: 762px; height: 41px;" /></p>

<ul>
	<li>Cách sắp xếp đầu tiên và cuối cùng có tổng khoảng cách Manhattan là <code>1 + 1 + 2 = 4</code>.</li>
	<li>Hai cách sắp xếp ở giữa có tổng khoảng cách Manhattan là <code>1 + 2 + 3 = 6</code>.</li>
</ul>

<p>Tổng khoảng cách Manhattan giữa mọi cặp quân cờ trong tất cả các cách sắp xếp là <code>4 + 6 + 6 + 4 = 20</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code><font face="monospace">2 &lt;= k &lt;= m * n</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta đặt $k$ quân cờ trên bàn cờ $m\times n$ và tính tổng khoảng cách Manhattan qua mọi cách sắp xếp. Số ô và $k$ đều có thể lên tới $10^5$, nên không thể liệt kê các tổ hợp.
>
> Khoảng cách Manhattan tách thành phần theo hàng và theo cột. Đóng góp bên trong một hàng (hoặc cột) chỉ phụ thuộc vào số ô được chọn trên đường đó; đóng góp giữa các đường phụ thuộc vào khoảng cách chỉ số và số cách hoàn tất việc đặt các quân cờ còn lại.
>
> Với các hàng $i<j$, số hạng là $(j-i)$ nhân với $n^2\,C_{mn-2}^{k-2}$ cùng với hệ số bội thông thường của cặp; các cột cũng tương tự. Sau khi lập các bảng tổ hợp, tổng có độ phức tạp $O(m+n)$.

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
