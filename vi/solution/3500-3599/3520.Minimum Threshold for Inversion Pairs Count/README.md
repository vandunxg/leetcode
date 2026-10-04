---
comments: true
difficulty: Medium
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Divide and Conquer
    - Merge Sort
---

<!-- problem:start -->

# [3520. Minimum Threshold for Inversion Pairs Count 🔒](https://leetcode.com/problems/minimum-threshold-for-inversion-pairs-count)

[中文文档](/solution/3500-3599/3520.Minimum%20Threshold%20for%20Inversion%20Pairs%20Count/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Một cặp nghịch thế với <strong>ngưỡng</strong> <code>x</code> được định nghĩa là một cặp chỉ số <code>(i, j)</code> thỏa mãn:</p>

<ul>
    <li><code>i &lt; j</code></li>
    <li><code>nums[i] &gt; nums[j]</code></li>
    <li>Hiệu giữa hai số <strong>không vượt quá</strong> <code>x</code> (tức là <code>nums[i] - nums[j] &lt;= x</code>).</li>
</ul>

<p>Nhiệm vụ của bạn là xác định số nguyên <strong>nhỏ nhất</strong> <code>min_threshold</code> sao cho có <strong>ít nhất</strong> <code>k</code> cặp nghịch thế với ngưỡng <code>min_threshold</code>.</p>

<p>Nếu không tồn tại số nguyên nào như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,3,2,1], k = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với ngưỡng <code>x = 2</code>, các cặp là:</p>

<ol>
    <li><code>(3, 4)</code>, trong đó <code>nums[3] == 4</code> và <code>nums[4] == 3</code>.</li>
    <li><code>(2, 5)</code>, trong đó <code>nums[2] == 3</code> và <code>nums[5] == 2</code>.</li>
    <li><code>(3, 5)</code>, trong đó <code>nums[3] == 4</code> và <code>nums[5] == 2</code>.</li>
    <li><code>(4, 5)</code>, trong đó <code>nums[4] == 3</code> và <code>nums[5] == 2</code>.</li>
    <li><code>(1, 6)</code>, trong đó <code>nums[1] == 2</code> và <code>nums[6] == 1</code>.</li>
    <li><code>(2, 6)</code>, trong đó <code>nums[2] == 3</code> và <code>nums[6] == 1</code>.</li>
    <li><code>(4, 6)</code>, trong đó <code>nums[4] == 3</code> và <code>nums[6] == 1</code>.</li>
    <li><code>(5, 6)</code>, trong đó <code>nums[5] == 2</code> và <code>nums[6] == 1</code>.</li>
</ol>

<p>Nếu chọn một số nguyên nhỏ hơn 2 làm ngưỡng, số cặp nghịch thế sẽ ít hơn <code>k</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,9,9,9,1], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với ngưỡng <code>x = 8</code>, các cặp là:</p>

<ol>
    <li><code>(0, 1)</code>, trong đó <code>nums[0] == 10</code> và <code>nums[1] == 9</code>.</li>
    <li><code>(0, 2)</code>, trong đó <code>nums[0] == 10</code> và <code>nums[2] == 9</code>.</li>
    <li><code>(0, 3)</code>, trong đó <code>nums[0] == 10</code> và <code>nums[3] == 9</code>.</li>
    <li><code>(1, 4)</code>, trong đó <code>nums[1] == 9</code> và <code>nums[4] == 1</code>.</li>
    <li><code>(2, 4)</code>, trong đó <code>nums[2] == 9</code> và <code>nums[4] == 1</code>.</li>
    <li><code>(3, 4)</code>, trong đó <code>nums[3] == 9</code> và <code>nums[4] == 1</code>.</li>
</ol>

<p>Nếu chọn một số nguyên nhỏ hơn 8 làm ngưỡng, số cặp nghịch thế sẽ ít hơn <code>k</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khi ngưỡng lớn hơn, chỉ các cặp nghịch thế có hiệu không vượt quá ngưỡng đó được thêm vào, vì vậy số lượng là đơn điệu. Với $n \le 10^4$, ta có thể dùng tìm kiếm nhị phân trên ngưỡng.
>
> Với một giá trị $x$ cần kiểm tra, Fenwick tree (hoặc merge sort) đếm các cặp có hiệu thuộc $(0, x]$. Giá trị $x$ nhỏ nhất có số cặp ít nhất là $k$ chính là đáp án.

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
