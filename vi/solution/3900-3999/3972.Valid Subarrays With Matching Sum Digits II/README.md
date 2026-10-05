---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3972. Valid Subarrays With Matching Sum Digits II 🔒](https://leetcode.com/problems/valid-subarrays-with-matching-sum-digits-ii)

[中文文档](/solution/3900-3999/3972.Valid%20Subarrays%20With%20Matching%20Sum%20Digits%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một chữ số nguyên <code>x</code>.</p>

<p>Một <span data-keyword="subarray-nonempty"><strong>mảng con</strong></span> <code>nums[l..r]</code> được gọi là <strong>hợp lệ</strong> nếu tổng các phần tử của nó thỏa mãn cả hai điều kiện sau:</p>

<ul>
	<li>Chữ số đầu tiên của tổng bằng <code>x</code>.</li>
	<li>Chữ số cuối cùng của tổng bằng <code>x</code>.</li>
</ul>

<p>Hãy trả về số lượng mảng con hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,100,1], x = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ là:</p>

<ul>
	<li><code>nums[0..0]</code>: <code>sum = 1</code></li>
	<li><code>nums[0..1]</code>: <code>sum = 1 + 100 = 101</code></li>
	<li><code>nums[1..2]</code>: <code>sum = 100 + 1 = 101</code></li>
	<li><code>nums[2..2]</code>: <code>sum = 1</code></li>
</ul>

<p>Do đó, đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1], x = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con duy nhất là <code>nums[0..0]</code>, có tổng bằng 1 nên không thỏa mãn các điều kiện.</p>

<p>Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= x &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ở Phần I, ta liệt kê các mảng con; còn bây giờ $n\le 10^5$. Chữ số cuối của tổng là hiệu của hai tổng tiền tố theo modulo $10$; chữ số đầu phụ thuộc vào độ lớn nên khó xử lý hơn.
>
> Chia các tổng tiền tố vào các nhóm theo phần dư modulo $10$. Với mỗi đầu phải, đếm các đầu trái thỏa mãn điều kiện về chữ số cuối và tổng đoạn có chữ số đầu là $x$, sau khi chia theo bậc độ lớn.
>
> Thư mục này chưa có lời giải được cài đặt; phần trình bày dừng ở tổng tiền tố modulo $10$ kết hợp với bộ lọc chữ số đầu.

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
