---
comments: true
difficulty: Hard
tags:
    - Math
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3916. Number of ZigZag Arrays III 🔒](https://leetcode.com/problems/number-of-zigzag-arrays-iii)

[中文文档](/solution/3900-3999/3916.Number%20of%20ZigZag%20Arrays%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <code>n</code>, <code>l</code> và <code>r</code>.</p>

<p>Một mảng <strong>ZigZag</strong> có độ dài <code>n</code> được định nghĩa như sau:</p>

<ul>
	<li>Mỗi phần tử nằm trong phạm vi <code>[l, r]</code>.</li>
	<li>Không có <strong>hai</strong> phần tử kề nhau nào bằng nhau.</li>
	<li>Không có <strong>ba</strong> phần tử liên tiếp nào tạo thành một dãy <strong>tăng nghiêm ngặt</strong> hoặc <strong>giảm nghiêm ngặt</strong>.</li>
</ul>

<p>Hãy trả về tổng số mảng <strong>ZigZag</strong> hợp lệ.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, l = 4, r = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có 2 mảng ZigZag hợp lệ có độ dài <code>n = 3</code> sử dụng các giá trị trong phạm vi <code>[4, 5]</code>:</p>

<ul>
	<li><code>[4, 5, 4]</code></li>
	<li><code>[5, 4, 5]</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, l = 1, r = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 10 mảng ZigZag hợp lệ có độ dài <code>n = 3</code> sử dụng các giá trị trong phạm vi <code>[1, 3]</code>:</p>

<ul>
	<li><code>[1, 2, 1]</code>, <code>[1, 3, 1]</code>, <code>[1, 3, 2]</code></li>
	<li><code>[2, 1, 2]</code>, <code>[2, 1, 3]</code>, <code>[2, 3, 1]</code>, <code>[2, 3, 2]</code></li>
	<li><code>[3, 1, 2]</code>, <code>[3, 1, 3]</code>, <code>[3, 2, 3]</code></li>
</ul>

<p>Tất cả các mảng đều thỏa mãn các điều kiện ZigZag.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 200</code></li>
	<li><code>1 &lt;= l &lt; r &lt;= 10<sup>​​​​​​​9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng giá trị có thể dài tới $10^9$ trong khi $n\le 200$, nên chúng ta không thể liệt kê các số cụ thể. Các ràng buộc ZigZag chỉ quan tâm đến mẫu tăng/giảm của ba phần tử liên tiếp, tức là thứ tự tương đối.
>
> Sau khi coi $[l,r]$ là một thứ tự toàn phần có độ dài $m=r-l+1$, một trạng thái là "giá trị trước đó cộng với hướng hiện tại". $m$ vẫn có thể rất lớn, nên các chuyển trạng thái trên các giá trị phải được biểu diễn bằng prefix sum hoặc ma trận.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần hướng dẫn dừng ở nhận xét rằng DP phải chạy trên thứ tự tương đối thay vì các giá trị thô.

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
