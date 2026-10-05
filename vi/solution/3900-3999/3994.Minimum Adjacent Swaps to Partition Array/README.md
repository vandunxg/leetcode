---
comments: true
difficulty: Medium
rating: 1704
source: Biweekly Contest 187 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3994. Minimum Adjacent Swaps to Partition Array](https://leetcode.com/problems/minimum-adjacent-swaps-to-partition-array)

[中文文档](/solution/3900-3999/3994.Minimum%20Adjacent%20Swaps%20to%20Partition%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>a</code> và <code>b</code> sao cho <code>a &lt; b</code>.</p>

<p>Một mảng được gọi là <strong>tốt</strong> nếu có thể chia thành ba phần <strong>liên tiếp</strong>, theo thứ tự sau:</p>

<ul>
	<li>Mọi phần tử trong phần thứ nhất đều <strong>nhỏ hơn</strong> <code>a</code>.</li>
	<li>Mọi phần tử trong phần thứ hai đều <strong>nằm trong</strong> đoạn <code>[a, b]</code>, kể cả hai đầu mút.</li>
	<li>Mọi phần tử trong phần thứ ba đều <strong>lớn hơn</strong> <code>b</code>.</li>
</ul>

<p>Một trong ba phần <strong>có thể</strong> rỗng.</p>

<p>Trong một <strong>phép đổi chỗ kề nhau</strong>, bạn có thể đổi chỗ hai phần tử <strong>liền kề</strong> của <code>nums</code>.</p>

<p>Trả về <strong>số phép đổi chỗ kề nhau nhỏ nhất</strong> cần thực hiện để <code>nums</code> trở thành mảng tốt. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2,4,5,6], a = 3, b = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đổi chỗ <code>nums[1]</code> và <code>nums[2]</code>. Mảng trở thành <code>[1, 2, 3, 4, 5, 6]</code>.</li>
	<li>Mảng này là mảng tốt vì có thể chia thành <code>[1, 2]</code>, <code>[3, 4]</code> và <code>[5, 6]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,7,5,3], a = 4, b = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi phép đổi chỗ tối ưu là:</p>

<ul>
	<li>Đổi chỗ <code>nums[2]</code> và <code>nums[3]</code>. Mảng trở thành <code>[9, 7, 3, 5]</code>.</li>
	<li>Đổi chỗ <code>nums[1]</code> và <code>nums[2]</code>. Mảng trở thành <code>[9, 3, 7, 5]</code>.</li>
	<li>Đổi chỗ <code>nums[0]</code> và <code>nums[1]</code>. Mảng trở thành <code>[3, 9, 7, 5]</code>.</li>
	<li>Đổi chỗ <code>nums[1]</code> và <code>nums[2]</code>. Mảng trở thành <code>[3, 7, 9, 5]</code>.</li>
	<li>Đổi chỗ <code>nums[2]</code> và <code>nums[3]</code>. Mảng trở thành <code>[3, 7, 5, 9]</code>.</li>
	<li>Mảng này là mảng tốt vì có thể chia thành <code>[3]</code>, <code>[7, 5]</code> và <code>[9]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,7,5,9], a = 4, b = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng đã là mảng tốt. Không cần thực hiện phép đổi chỗ nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>​​​​​​​1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= a &lt; b &lt;= 10<sup>9</sup>​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Số phép đổi chỗ kề nhau chính là số nghịch thế so với thứ tự đích. Ba nhóm giá trị là $(-\infty,a)$, $[a,b]$, $(b,+\infty)$ và phải xuất hiện theo đúng thứ tự đó; thứ tự bên trong mỗi nhóm có thể giữ nguyên.
>
> Ánh xạ mỗi giá trị thành một loại $0/1/2$. Sắp xếp chuỗi loại này bằng các phép đổi chỗ kề nhau có chi phí bằng số nghịch thế giữa các loại, có thể đếm bằng Fenwick tree.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng ở việc đếm nghịch thế của ba loại.

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
