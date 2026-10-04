---
comments: true
difficulty: Hard
rating: 2722
source: Weekly Contest 427 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Geometry
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [3382. Maximum Area Rectangle With Point Constraints II](https://leetcode.com/problems/maximum-area-rectangle-with-point-constraints-ii)

[中文文档](/solution/3300-3399/3382.Maximum%20Area%20Rectangle%20With%20Point%20Constraints%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có n điểm trên một mặt phẳng vô hạn. Cho hai mảng số nguyên <code>xCoord</code> và <code>yCoord</code>, trong đó <code>(xCoord[i], yCoord[i])</code> biểu diễn tọa độ của điểm thứ <code>i<sup>th</sup></code>.</p>

<p>Nhiệm vụ của bạn là tìm <strong>diện tích lớn nhất</strong> của một hình chữ nhật thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Có thể được tạo thành bằng cách dùng <strong>bốn</strong> điểm trong số các điểm đã cho làm các đỉnh.</li>
	<li><strong>Không</strong> chứa bất kỳ điểm nào khác bên trong hoặc trên biên.</li>
	<li>Có các cạnh <strong>song song</strong> với các trục tọa độ.</li>
</ul>

<p>Trả về <strong>diện tích lớn nhất</strong> có thể đạt được, hoặc -1 nếu không thể tạo ra hình chữ nhật nào như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">xCoord = [1,1,3,3], yCoord = [1,3,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="Example 1 diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3382.Maximum%20Area%20Rectangle%20With%20Point%20Constraints%20II/images/example1.png" style="width: 229px; height: 228px;" /></strong></p>

<p>Ta có thể tạo một hình chữ nhật với 4 điểm này làm các đỉnh và không có điểm nào khác nằm bên trong hoặc trên biên. Do đó, diện tích lớn nhất có thể đạt được là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">xCoord = [1,1,3,3,2], yCoord = [1,3,1,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="Example 2 diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3382.Maximum%20Area%20Rectangle%20With%20Point%20Constraints%20II/images/example2.png" style="width: 229px; height: 228px;" /></strong></p>

<p>Chỉ có một hình chữ nhật có thể tạo thành từ các điểm <code>[1,1], [1,3], [3,1]</code> và <code>[3,3]</code>, nhưng điểm <code>[2,2]</code> luôn nằm bên trong hình chữ nhật đó. Vì vậy, kết quả trả về là -1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">xCoord = [1,1,3,3,1,3], yCoord = [1,3,1,3,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="Example 3 diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3382.Maximum%20Area%20Rectangle%20With%20Point%20Constraints%20II/images/example3.png" style="width: 229px; height: 228px;" /></strong></p>

<p>Hình chữ nhật có diện tích lớn nhất được tạo bởi các điểm <code>[1,3], [1,2], [3,2], [3,3]</code>, với diện tích bằng 2. Ngoài ra, các điểm <code>[1,1], [1,2], [3,1], [3,2]</code> cũng tạo thành một hình chữ nhật hợp lệ có cùng diện tích.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= xCoord.length == yCoord.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>0 &lt;= xCoord[i], yCoord[i]&nbsp;&lt;= 8 * 10<sup>7</sup></code></li>
	<li>Tất cả các điểm đã cho đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc này giống phần I, nhưng $n \le 2 \times 10^5$, nên không thể liệt kê các cặp góc đối diện. Một hình chữ nhật hợp lệ có đúng một điểm bên trái và một điểm bên phải trên mỗi cạnh ngang, đồng thời không có điểm nào bên trong.
>
> Ta quét theo $x$, lưu điểm trước đó của mỗi $y$, rồi kiểm tra xem hộp ứng viên có rỗng hay không bằng Fenwick tree hoặc segment tree.
>
> Với mỗi ứng viên rỗng, ta cập nhật diện tích lớn nhất; nếu không có ứng viên nào, trả về $-1$.

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
