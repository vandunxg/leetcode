---
comments: true
difficulty: Hard
rating: 2419
source: Weekly Contest 457 Q4
tags:
    - Math
---

<!-- problem:start -->

# [3609. Minimum Moves to Reach Target in Grid](https://leetcode.com/problems/minimum-moves-to-reach-target-in-grid)

[中文文档](/solution/3600-3699/3609.Minimum%20Moves%20to%20Reach%20Target%20in%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho bốn số nguyên <code>sx</code>, <code>sy</code>, <code>tx</code> và <code>ty</code>, biểu diễn hai điểm <code>(sx, sy)</code> và <code>(tx, ty)</code> trên một lưới 2D vô hạn.</p>

<p>Bạn bắt đầu tại <code>(sx, sy)</code>.</p>

<p>Tại bất kỳ điểm nào <code>(x, y)</code>, đặt <code>m = max(x, y)</code>. Bạn có thể thực hiện một trong hai thao tác:</p>

<ul>
    <li>Di chuyển đến <code>(x + m, y)</code>, hoặc</li>
    <li>Di chuyển đến <code>(x, y + m)</code>.</li>
</ul>

<p>Trả về số bước di chuyển <strong>nhỏ nhất</strong> cần thực hiện để đến <code>(tx, ty)</code>. Nếu không thể đến đích, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">sx = 1, sy = 2, tx = 5, ty = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<ul>
    <li>Bước 1: <code>max(1, 2) = 2</code>. Tăng tọa độ y thêm 2, di chuyển từ <code>(1, 2)</code> đến <code>(1, 2 + 2) = (1, 4)</code>.</li>
    <li>Bước 2: <code>max(1, 4) = 4</code>. Tăng tọa độ x thêm 4, di chuyển từ <code>(1, 4)</code> đến <code>(1 + 4, 4) = (5, 4)</code>.</li>
</ul>

<p>Vậy số bước di chuyển nhỏ nhất để đến <code>(5, 4)</code> là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">sx = 0, sy = 1, tx = 2, ty = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<ul>
    <li>Bước 1: <code>max(0, 1) = 1</code>. Tăng tọa độ x thêm 1, di chuyển từ <code>(0, 1)</code> đến <code>(0 + 1, 1) = (1, 1)</code>.</li>
    <li>Bước 2: <code>max(1, 1) = 1</code>. Tăng tọa độ x thêm 1, di chuyển từ <code>(1, 1)</code> đến <code>(1 + 1, 1) = (2, 1)</code>.</li>
    <li>Bước 3: <code>max(2, 1) = 2</code>. Tăng tọa độ y thêm 2, di chuyển từ <code>(2, 1)</code> đến <code>(2, 1 + 2) = (2, 3)</code>.</li>
</ul>

<p>Vậy số bước di chuyển nhỏ nhất để đến <code>(2, 3)</code> là 3.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">sx = 1, sy = 1, tx = 2, ty = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Không thể đi từ <code>(1, 1)</code> đến <code>(2, 2)</code> bằng các thao tác được phép. Vì vậy, đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>0 &lt;= sx &lt;= tx &lt;= 10<sup>9</sup></code></li>
    <li><code>0 &lt;= sy &lt;= ty &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tọa độ có thể đạt tới $10^9$ và mỗi bước cộng thêm $\max(x,y)$ vào một trục, vì vậy không thể dùng tìm kiếm xuôi để liệt kê không gian trạng thái. Thao tác này có thể đảo ngược: nếu một tọa độ lớn hơn hẳn tọa độ còn lại, thì lần cộng cuối cùng phải được thực hiện trên trục đó.
>
> Ta đi ngược từ $(t_x,t_y)$ về $(s_x,s_y)$. Khi $t_x\ge t_y$, nếu $t_x\ge 2t_y$ thì trừ đi một bội của $t_y$ (tương ứng với nhiều lần cộng vào $x$); nếu không thì trừ $t_y$ một lần. Trường hợp $t_y>t_x$ được xử lý tương tự.
>
> Nếu một bước khiến tọa độ nhỏ hơn tọa độ ban đầu, hoặc hai tọa độ bằng nhau trước khi đạt đến điểm ban đầu, thì không thể đến đích. Mỗi bước đi ngược là duy nhất, nên số bước đếm được chính là nhỏ nhất.

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
