---
comments: true
difficulty: Hard
rating: 2611
source: Weekly Contest 505 Q4
tags:
    - Queue
    - Array
    - Binary Search
    - Dynamic Programming
    - Prefix Sum
    - Sliding Window
    - Monotonic Queue
---

<!-- problem:start -->

# [3957. Maximum Sum of M Non-Overlapping Subarrays II](https://leetcode.com/problems/maximum-sum-of-m-non-overlapping-subarrays-ii)

[中文文档](/solution/3900-3999/3957.Maximum%20Sum%20of%20M%20Non-Overlapping%20Subarrays%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, cùng ba số nguyên <code>m</code>, <code>l</code> và <code>r</code>.</p>

<p>Nhiệm vụ của bạn là chọn <strong>ít nhất</strong> một và <strong>nhiều nhất</strong> <code>m</code> <strong><span data-keyword="subarray-nonempty">mảng con</span> không giao nhau</strong> từ <code>nums</code> sao cho:</p>

<ul>
	<li>Mỗi <strong>mảng con</strong> được chọn có độ dài nằm trong khoảng <code>[l, r]</code> (bao gồm hai đầu mút).</li>
	<li>Tổng của tất cả các <strong>mảng con</strong> được chọn là <strong>lớn nhất</strong>.</li>
</ul>

<p>Trả về <strong>tổng lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1,-5,2], m = 2, l = 1, r = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chiến lược tối ưu là:</p>

<ul>
	<li>Chọn mảng con <code>[4, 1]</code> có tổng <code>4 + 1 = 5</code> và mảng con <code>[2]</code> có tổng bằng 2. Cả hai mảng con đều có độ dài nằm trong khoảng <code>[l, r]</code>.</li>
	<li>Tổng của các mảng con này là <code>5 + 2 = 7</code>, đây là tổng lớn nhất có thể đạt được khi chọn nhiều nhất <code>m = 2</code> mảng con.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,3,4], m = 2, l = 1, r = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chiến lược tối ưu là:</p>

<ul>
	<li>Chọn mảng con <code>[1]</code> có tổng <code>1</code> và mảng con <code>[3, 4]</code> có tổng <code>3 + 4 = 7</code>. Cả hai mảng con đều có độ dài nằm trong khoảng <code>[l, r]</code>.</li>
	<li>Tổng của các mảng con này là <code>1 + 7 = 8</code>, đây là tổng lớn nhất có thể đạt được khi chọn nhiều nhất <code>m = 2</code> mảng con.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,7,-4], m = 1, l = 2, r = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn mảng con <code>[-1, 7]</code> từ <code>nums</code>, có độ dài nằm trong khoảng <code>[l, r]</code>.</li>
	<li>Tổng của mảng con này là <code>-1 + 7 = 6</code>, đây là tổng lớn nhất có thể đạt được khi chọn nhiều nhất <code>m = 1</code> mảng con.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-3,-4,-1], m = 2, l = 1, r = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tất cả các mảng con của <code>nums</code> đều có tổng âm. Chiến lược tối ưu là chọn mảng con <code>[-1]</code>, có độ dài nằm trong khoảng <code>[l, r]</code>.</li>
	<li>Tổng của mảng con này là -1, đây là tổng lớn nhất có thể đạt được khi chọn nhiều nhất <code>m = 2</code> mảng con.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup>​​​​​​​</code></li>
	<li><code>1 &lt;= m &lt;= n</code></li>
	<li><code>1 &lt;= l &lt;= r &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đề bài giống phần I nhưng có giới hạn chặt hơn, nên DP duyệt theo độ dài đoạn không còn đủ nhanh. Độ dài đoạn cuối trong $[l,r]$ phải được chuyển thành phép lấy giá trị lớn nhất trên một cửa sổ trượt của các tổng tiền tố, để mỗi lớp chạy trong $O(n)$.
>
> Một hàng đợi đơn điệu lưu $f[j][t-1]-s_j$ trên cửa sổ $j$ hợp lệ sẽ cho độ phức tạp $O(nm)$. Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng ở việc tăng tốc phép chuyển của phần I.

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
