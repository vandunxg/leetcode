---
comments: true
difficulty: Hard
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3944. Minimum Operations to Make Array Modulo Alternating II 🔒](https://leetcode.com/problems/minimum-operations-to-make-array-modulo-alternating-ii)

[中文文档](/solution/3900-3999/3944.Minimum%20Operations%20to%20Make%20Array%20Modulo%20Alternating%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể <strong>tăng</strong> hoặc <strong>giảm</strong> một phần tử bất kỳ của <code>nums</code> đi 1.</p>

<p>Một mảng được gọi là <strong>modulo luân phiên</strong> nếu tồn tại hai số nguyên <strong>phân biệt</strong> <code>x</code> và <code>y</code> (<code>0 &lt;= x, y &lt; k</code>) sao cho:</p>

<ul>
	<li>Với mọi chỉ số <strong>chẵn</strong> <code>i</code>, <code>nums[i] % k == x</code></li>
	<li>Với mọi chỉ số <strong>lẻ</strong> <code>i</code>, <code>nums[i] % k == y</code></li>
</ul>

<p>Trả về số thao tác <strong>ít nhất</strong> cần thực hiện để biến <code>nums</code> thành mảng <strong>modulo luân phiên</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,2,8], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>x = 1</code> cho các chỉ số chẵn và <code>y = 2</code> cho các chỉ số lẻ.</li>
	<li>Thực hiện các thao tác sau:
	<ul>
		<li>Tăng <code>nums[1] = 4</code> lên 1, được <code>nums = [1, 5, 2, 8]</code>.</li>
		<li>Giảm <code>nums[2] = 2</code> đi 1, được <code>nums = [1, 5, 1, 8]</code>.</li>
	</ul>
	</li>
	<li>Lúc này, với các chỉ số chẵn, <code>nums[i] % k = 1</code>, còn với các chỉ số lẻ, <code>nums[i] % k = 2</code>.</li>
	<li>Do đó, tổng số thao tác cần thực hiện là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tăng <code>nums[1]</code> lên 1 được <code>nums = [1, 2, 1]</code>, thỏa mãn điều kiện với <code>x = 1</code> và <code>y = 2</code>.</li>
	<li>Do đó, tổng số thao tác cần thực hiện là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,7,8], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng đã thỏa mãn điều kiện với <code>x = 0</code> và <code>y = 1</code>. Do đó, không cần thực hiện thao tác nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>2 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mục tiêu giống phần I, nhưng giờ đây $n,k\le 10^5$, nên không thể duyệt mọi cặp $(x,y)$. Chi phí đưa một giá trị về phần dư $t$ là khoảng cách trên vòng tròn và chỉ phụ thuộc vào $v\bmod k$.
>
> Ta cộng dồn các chi phí này riêng cho các chỉ số chẵn và lẻ. Khi đó, cặp tốt nhất $x\neq y$ là sự kết hợp của phần dư nhỏ nhất và nhỏ thứ hai ở mỗi phía, có thể tìm được trong $O(k)$ thay vì $O(k^2)$.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần walkthrough dừng ở việc cộng dồn chi phí phần dư theo tính chẵn lẻ.

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
