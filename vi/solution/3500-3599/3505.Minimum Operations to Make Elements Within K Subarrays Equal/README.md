---
comments: true
difficulty: Hard
rating: 2538
source: Weekly Contest 443 Q4
tags:
    - Array
    - Hash Table
    - Math
    - Dynamic Programming
    - Sliding Window
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3505. Minimum Operations to Make Elements Within K Subarrays Equal](https://leetcode.com/problems/minimum-operations-to-make-elements-within-k-subarrays-equal)

[中文文档](/solution/3500-3599/3505.Minimum%20Operations%20to%20Make%20Elements%20Within%20K%20Subarrays%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code> và hai số nguyên, <code>x</code> và <code>k</code>. Bạn có thể thực hiện thao tác sau bất kỳ số lần nào (<strong>kể cả không lần nào</strong>):</p>

<ul>
    <li>Tăng hoặc giảm bất kỳ phần tử nào của <code>nums</code> đi 1.</li>
</ul>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thiết để có <strong>ít nhất</strong> <code>k</code> <em><span data-keyword="subarray-nonempty">mảng con</span> không chồng lấn</em> có kích thước <strong>chính xác</strong> bằng <code>x</code> trong <code>nums</code>, sao cho tất cả phần tử trong mỗi mảng con đều bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,-2,1,3,7,3,6,4,-1], x = 3, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Thực hiện 3 thao tác để cộng 3 vào <code>nums[1]</code> và 2 thao tác để trừ 2 khỏi <code>nums[3]</code>. Mảng sau đó là <code>[5, 1, 1, 1, 7, 3, 6, 4, -1]</code>.</li>
    <li>Thực hiện 1 thao tác để cộng 1 vào <code>nums[5]</code> và 2 thao tác để trừ 2 khỏi <code>nums[6]</code>. Mảng sau đó là <code>[5, 1, 1, 1, 7, 4, 4, 4, -1]</code>.</li>
    <li>Lúc này, tất cả phần tử trong mỗi mảng con <code>[1, 1, 1]</code> (từ chỉ số 1 đến 3) và <code>[4, 4, 4]</code> (từ chỉ số 5 đến 7) đều bằng nhau. Vì đã sử dụng tổng cộng 8 thao tác, đầu ra là 8.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,-2,-2,-2,1,5], x = 2, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Thực hiện 3 thao tác để trừ 3 khỏi <code>nums[4]</code>. Mảng sau đó là <code>[9, -2, -2, -2, -2, 5]</code>.</li>
    <li>Lúc này, tất cả phần tử trong mỗi mảng con <code>[-2, -2]</code> (từ chỉ số 1 đến 2) và <code>[-2, -2]</code> (từ chỉ số 3 đến 4) đều bằng nhau. Vì đã sử dụng 3 thao tác, đầu ra là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>-10<sup>6</sup> &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
    <li><code>2 &lt;= x &lt;= nums.length</code></li>
    <li><code>1 &lt;= k &lt;= 15</code></li>
    <li><code>2 &lt;= k * x &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 10^5$ và $k \le 15$, nên không thể vét cạn mọi cửa sổ có độ dài $x$ rồi tìm các tổ hợp. Chi phí để làm các phần tử trong một cửa sổ bằng nhau là tổng khoảng cách đến median, và có thể tính trước cho mọi vị trí bắt đầu.
>
> Việc chọn $k$ cửa sổ không chồng lấn là một bài toán knapsack theo nhóm: $f[i][j]$ là chi phí nhỏ nhất khi sử dụng $j$ cửa sổ trong $i$ vị trí đầu tiên. Mỗi bước hoặc bỏ qua, hoặc đặt một cửa sổ kết thúc tại $i$. Giá trị $k$ nhỏ giúp DP khả thi.

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
