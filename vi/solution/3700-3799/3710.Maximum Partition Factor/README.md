---
comments: true
difficulty: Hard
rating: 2135
source: Biweekly Contest 167 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3710. Maximum Partition Factor](https://leetcode.com/problems/maximum-partition-factor)

[中文文档](/solution/3700-3799/3710.Maximum%20Partition%20Factor/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn tọa độ của điểm <code><font>i<sup>th</sup></font></code> trên mặt phẳng Descartes.</p>

<p><strong>Khoảng cách Manhattan</strong> giữa hai điểm <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> và <code>points[j] = [x<sub>j</sub>, y<sub>j</sub>]</code> là <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.</p>

<p>Chia <code>n</code> điểm thành <strong>đúng hai nhóm không rỗng</strong>. <strong>Hệ số phân hoạch</strong> của một cách chia là <strong>khoảng cách Manhattan nhỏ nhất</strong> giữa mọi cặp điểm không có thứ tự nằm trong cùng một nhóm.</p>

<p>Trả về <strong>hệ số phân hoạch</strong> <strong>lớn nhất</strong> có thể đạt được trong tất cả các cách chia hợp lệ.</p>

<p>Lưu ý: Nhóm có kích thước 1 không đóng góp cặp điểm nào trong nhóm. Khi <code>n = 2</code> (cả hai nhóm đều có kích thước 1), không có cặp điểm nào trong nhóm, nên quy ước hệ số phân hoạch là 0.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span>points = [[0,0],[0,2],[2,0],[2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span>4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta chia các điểm thành hai nhóm: <code>{[0, 0], [2, 2]}</code> và <code>{[0, 2], [2, 0]}</code>.</p>

<ul>
	<li>
	<p>Trong nhóm đầu tiên, cặp duy nhất có khoảng cách Manhattan là <code>|0 - 2| + |0 - 2| = 4</code>.</p>
	</li>
	<li>
	<p>Trong nhóm thứ hai, cặp duy nhất cũng có khoảng cách là <code>|0 - 2| + |2 - 0| = 4</code>.</p>
	</li>
</ul>

<p>Hệ số phân hoạch của cách chia này là <code>min(4, 4) = 4</code>, đây là giá trị lớn nhất.</p>
</div>

<p><strong>Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span>points = [[0,0],[0,1],[10,0]]</span></p>

<p><strong>Đầu ra:</strong> <span>11</span></p>

<p><strong>Giải thích:​​​​​​​</strong></p>

<p>Ta chia các điểm thành hai nhóm: <code>{[0, 1], [10, 0]}</code> và <code>{[0, 0]}</code>.</p>

<ul>
	<li>
	<p>Trong nhóm đầu tiên, cặp duy nhất có khoảng cách Manhattan là <code>|0 - 10| + |1 - 0| = 11</code>.</p>
	</li>
	<li>
	<p>Nhóm thứ hai chỉ có một điểm, nên không đóng góp cặp điểm nào.</p>
	</li>
</ul>

<p>Hệ số phân hoạch của cách chia này là <code>11</code>, đây là giá trị lớn nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= points.length &lt;= 500</code></li>
	<li><code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code></li>
	<li><code>-10<sup>8</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hệ số phân hoạch là khoảng cách Manhattan nhỏ nhất giữa các điểm trong cùng nhóm, còn mục tiêu là tối đa hóa giá trị này, nên binary search trên đáp án là một lựa chọn tự nhiên. Với một giá trị ứng viên $d$, hai điểm cách nhau nhỏ hơn $d$ không thể nằm cùng một nhóm; khi đó chỉ cần kiểm tra xem đồ thị gồm các cạnh có độ dài nhỏ hơn $d$ có phải là đồ thị hai phía hay không.

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
