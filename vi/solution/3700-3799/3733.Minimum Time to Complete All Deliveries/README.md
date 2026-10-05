---
comments: true
difficulty: Medium
rating: 1972
source: Weekly Contest 474 Q3
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [3733. Minimum Time to Complete All Deliveries](https://leetcode.com/problems/minimum-time-to-complete-all-deliveries)

[中文文档](/solution/3700-3799/3733.Minimum%20Time%20to%20Complete%20All%20Deliveries/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên có kích thước 2: <code>d = [d<sub>1</sub>, d<sub>2</sub>]</code> và <code>r = [r<sub>1</sub>, r<sub>2</sub>]</code>.</p>

<p>Hai drone giao hàng được giao hoàn thành một số lượng đơn hàng cụ thể. Drone <code>i</code> phải hoàn thành <code>d<sub>i</sub></code> lượt giao hàng.</p>

<p>Mỗi lượt giao hàng mất <strong>chính xác</strong> một giờ và tại mỗi giờ <strong>chỉ một</strong> drone có thể giao hàng.</p>

<p>Ngoài ra, cả hai drone cần sạc lại theo các khoảng thời gian cụ thể, trong thời gian đó chúng không thể giao hàng. Drone <code>i</code> phải sạc lại sau mỗi <code>r<sub>i</sub></code> giờ (tức là vào các giờ là bội số của <code>r<sub>i</sub></code>).</p>

<p>Trả về một số nguyên biểu thị <strong>tổng thời gian nhỏ nhất</strong> (tính bằng giờ) cần để hoàn thành tất cả lượt giao hàng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">d = [3,1], r = [2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Drone thứ nhất giao hàng vào các giờ 1, 3, 5 (sạc lại vào các giờ 2, 4).</li>
	<li>Drone thứ hai giao hàng vào giờ 2 (sạc lại vào giờ 3).</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">d = [1,3], r = [2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Drone thứ nhất giao hàng vào giờ 3 (sạc lại vào các giờ 2, 4, 6).</li>
	<li>Drone thứ hai giao hàng vào các giờ 1, 5, 7 (sạc lại vào các giờ 2, 4, 6).</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">d = [2,1], r = [3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Drone thứ nhất giao hàng vào các giờ 1, 2 (sạc lại vào giờ 3).</li>
	<li>Drone thứ hai giao hàng vào giờ 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
<li><code>d = [d<sub>1</sub>, d<sub>2</sub>]</code></li>
<li><code>1 &lt;= d<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
<li><code>r = [r<sub>1</sub>, r<sub>2</sub>]</code></li>
<li><code>2 &lt;= r<sub>i</sub> &lt;= 3 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ một drone có thể giao hàng trong một giờ, và drone $i$ phải sạc vào các bội số của $r_i$. Thời gian khả thi tăng theo nhu cầu, nên ta dùng tìm kiếm nhị phân trên tổng số giờ $t$. Drone $i$ có $t-\lfloor t/r_i\rfloor$ giờ rảnh; ta kiểm tra xem tổng thời gian rảnh của hai drone có đủ $d_1+d_2$ hay không, đồng thời mỗi drone có đủ cho $d_i$ của mình hay không.

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
