---
comments: true
difficulty: Hard
rating: 3124
source: Weekly Contest 475 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3743. Maximize Cyclic Partition Score](https://leetcode.com/problems/maximize-cyclic-partition-score)

[中文文档](/solution/3700-3799/3743.Maximize%20Cyclic%20Partition%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>vòng</strong> <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Hãy <strong>chia</strong> <code>nums</code> thành <strong>nhiều nhất</strong> <code>k</code><strong> </strong><span data-keyword="subarray-nonempty">mảng con</span>. Vì <code>nums</code> là mảng vòng, các mảng con này có thể nối từ cuối mảng quay lại đầu mảng.</p>

<p><strong>Độ rộng</strong> của một mảng con là hiệu giữa giá trị <strong>lớn nhất</strong> và <strong>nhỏ nhất</strong> của nó. <strong>Điểm số</strong> của một phép chia là tổng <strong>độ rộng</strong> của các mảng con.</p>

<p>Hãy trả về <strong>điểm số</strong> <strong>lớn nhất</strong> có thể đạt được trong tất cả các phép chia vòng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>nums</code> thành <code>[2, 3]</code> và <code>[3, 1]</code> (nối vòng).</li>
	<li>Độ rộng của <code>[2, 3]</code> là <code>max(2, 3) - min(2, 3) = 3 - 2 = 1</code>.</li>
	<li>Độ rộng của <code>[3, 1]</code> là <code>max(3, 1) - min(3, 1) = 3 - 1 = 2</code>.</li>
	<li>Điểm số là <code>1 + 2 = 3</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,3], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>nums</code> thành <code>[1, 2, 3, 3]</code>.</li>
	<li>Độ rộng của <code>[1, 2, 3, 3]</code> là <code>max(1, 2, 3, 3) - min(1, 2, 3, 3) = 3 - 1 = 2</code>.</li>
	<li>Điểm số là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,3], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giống Ví dụ 1, ta chia <code>nums</code> thành <code>[2, 3]</code> và <code>[3, 1]</code>. Lưu ý rằng <code>nums</code> có thể được chia thành ít hơn <code>k</code> mảng con.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng có dạng vòng và ta được dùng nhiều nhất $k$ đoạn; điểm số là tổng $\max-\min$ trên các đoạn. Vì $n\le 1000$, ta có thể cắt vòng tại từng vị trí bắt đầu, sau đó chạy interval DP để chia mảng tuyến tính thành nhiều nhất $k$ phần sao cho tổng độ rộng là lớn nhất.

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
