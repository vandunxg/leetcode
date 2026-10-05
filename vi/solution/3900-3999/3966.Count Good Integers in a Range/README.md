---
comments: true
difficulty: Hard
rating: 2006
source: Biweekly Contest 185 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3966. Count Good Integers in a Range](https://leetcode.com/problems/count-good-integers-in-a-range)

[中文文档](/solution/3900-3999/3966.Count%20Good%20Integers%20in%20a%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <code>l</code>, <code>r</code> và <code>k</code>.</p>

<p>Một số được gọi là <strong>tốt</strong> nếu <strong>độ lệch tuyệt đối</strong> giữa mọi cặp chữ số <strong>liền kề</strong> <strong>không vượt quá</strong> <code>k</code>.</p>

<p>Trả về số lượng số nguyên <strong>tốt</strong> trong đoạn <code>[l, r]</code> (bao gồm cả hai đầu mút).</p>

<p><strong>Độ lệch tuyệt đối</strong> giữa hai giá trị <code>x</code> và <code>y</code> được định nghĩa là <code>abs(x - y)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 10, r = 15, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các số nguyên tốt trong đoạn là 10, 11 và 12.</li>
	<li>Với 10, <code>abs(1 - 0) = 1</code>.</li>
	<li>Với 11, <code>abs(1 - 1) = 0</code>.</li>
	<li>Với 12, <code>abs(1 - 2) = 1</code>.</li>
	<li>Tất cả các độ lệch này đều không vượt quá <code>k = 1</code>. Vì vậy, đáp án là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 201, r = 204, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các số nguyên tốt trong đoạn là 201 và 202.</li>
	<li>Với 201, <code>abs(2 - 0) = 2</code> và <code>abs(0 - 1) = 1</code>.</li>
	<li>Với 202, <code>abs(2 - 0) = 2</code> và <code>abs(0 - 2) = 2</code>.</li>
	<li>Vì vậy, đáp án là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>10 &lt;= l &lt;= r &lt;= 10<sup>15</sup></code></li>
	<li><code>0 &lt;= k &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $l$ và $r$ có thể đạt tới $10^{15}$, nên không thể liệt kê các số nguyên. Một số tốt có các chữ số liền kề chênh lệch không quá $k$, đây là một ràng buộc phù hợp với digit DP.
>
> Đếm các số trong $[0,r]$ rồi trừ đi số lượng trong $[0,l-1]$. State lưu vị trí, chữ số trước đó, trạng thái đã chạm cận trên và trạng thái vẫn còn leading zero. Sau khi kết thúc leading zero, chữ số mới phải có độ lệch so với chữ số trước đó không quá $k$.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần tư duy dừng ở digit DP đó.

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
