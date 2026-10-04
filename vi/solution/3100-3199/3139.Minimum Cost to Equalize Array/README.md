---
comments: true
difficulty: Hard
rating: 2666
source: Weekly Contest 396 Q4
tags:
    - Greedy
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3139. Minimum Cost to Equalize Array](https://leetcode.com/problems/minimum-cost-to-equalize-array)

[中文文档](/solution/3100-3199/3139.Minimum%20Cost%20to%20Equalize%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code> và hai số nguyên <code>cost1</code> và <code>cost2</code>. Bạn được phép thực hiện <strong>một trong hai</strong> thao tác sau <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> trong <code>nums</code> và <strong>tăng</strong> <code>nums[i]</code> lên <code>1</code> với chi phí là <code>cost1</code>.</li>
	<li>Chọn hai chỉ số <strong>khác nhau</strong> <code>i</code>, <code>j</code> trong <code>nums</code> và <strong>tăng</strong> <code>nums[i]</code> và <code>nums[j]</code> lên <code>1</code> với chi phí là <code>cost2</code>.</li>
</ul>

<p>Trả về <strong>chi phí</strong> <strong>nhỏ nhất</strong> cần thiết để làm cho tất cả các phần tử trong mảng <strong>bằng nhau</strong><em>. </em></p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1], cost1 = 5, cost2 = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích: </strong></p>

<p>Có thể thực hiện các thao tác sau để làm cho các giá trị bằng nhau:</p>

<ul>
	<li>Tăng <code>nums[1]</code> lên 1 với chi phí là 5. <code>nums</code> trở thành <code>[4,2]</code>.</li>
	<li>Tăng <code>nums[1]</code> lên 1 với chi phí là 5. <code>nums</code> trở thành <code>[4,3]</code>.</li>
	<li>Tăng <code>nums[1]</code> lên 1 với chi phí là 5. <code>nums</code> trở thành <code>[4,4]</code>.</li>
</ul>

<p>Tổng chi phí là 15.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,3,3,5], cost1 = 2, cost2 = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích: </strong></p>

<p>Có thể thực hiện các thao tác sau để làm cho các giá trị bằng nhau:</p>

<ul>
	<li>Tăng <code>nums[0]</code> và <code>nums[1]</code> lên 1 với chi phí là 1. <code>nums</code> trở thành <code>[3,4,3,3,5]</code>.</li>
	<li>Tăng <code>nums[0]</code> và <code>nums[2]</code> lên 1 với chi phí là 1. <code>nums</code> trở thành <code>[4,4,4,3,5]</code>.</li>
	<li>Tăng <code>nums[0]</code> và <code>nums[3]</code> lên 1 với chi phí là 1. <code>nums</code> trở thành <code>[5,4,4,4,5]</code>.</li>
	<li>Tăng <code>nums[1]</code> và <code>nums[2]</code> lên 1 với chi phí là 1. <code>nums</code> trở thành <code>[5,5,5,4,5]</code>.</li>
	<li>Tăng <code>nums[3]</code> lên 1 với chi phí là 2. <code>nums</code> trở thành <code>[5,5,5,5,5]</code>.</li>
</ul>

<p>Tổng chi phí là 6.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5,3], cost1 = 1, cost2 = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể thực hiện các thao tác sau để làm cho các giá trị bằng nhau:</p>

<ul>
	<li>Tăng <code>nums[0]</code> lên 1 với chi phí là 1. <code>nums</code> trở thành <code>[4,5,3]</code>.</li>
	<li>Tăng <code>nums[0]</code> lên 1 với chi phí là 1. <code>nums</code> trở thành <code>[5,5,3]</code>.</li>
	<li>Tăng <code>nums[2]</code> lên 1 với chi phí là 1. <code>nums</code> trở thành <code>[5,5,4]</code>.</li>
	<li>Tăng <code>nums[2]</code> lên 1 với chi phí là 1. <code>nums</code> trở thành <code>[5,5,5]</code>.</li>
</ul>

<p>Tổng chi phí là 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= cost1 &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= cost2 &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các phần tử chỉ có thể tăng, nên giá trị đích chung ít nhất phải bằng giá trị lớn nhất hiện tại. Với $n\le 10^5$, ta không thể mô phỏng từng lần tăng.
>
> Khi $2\cdot cost1\le cost2$, thao tác theo cặp không bao giờ có lợi và chi phí là tổng độ thiếu hụt nhân với $cost1$. Ngược lại, các độ thiếu hụt nên được ghép cặp, trừ khi độ thiếu hụt lớn nhất quá lớn để có thể ghép tự do.
>
> Việc tăng thêm giá trị đích có thể cải thiện khả năng ghép cặp và chỉ cần xét một số hữu hạn mức tăng thêm. Với mỗi ứng viên, chuyển tổng độ thiếu hụt và độ thiếu hụt lớn nhất thành số lượng thao tác, rồi lấy chi phí nhỏ nhất theo modulo $10^9+7$.

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
