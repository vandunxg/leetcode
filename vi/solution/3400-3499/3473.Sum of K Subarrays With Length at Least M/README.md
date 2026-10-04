---
comments: true
difficulty: Medium
rating: 2274
source: Weekly Contest 439 Q3
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3473. Sum of K Subarrays With Length at Least M](https://leetcode.com/problems/sum-of-k-subarrays-with-length-at-least-m)

[中文文档](/solution/3400-3499/3473.Sum%20of%20K%20Subarrays%20With%20Length%20at%20Least%20M/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>k</code> và <code>m</code>.</p>

<p>Trả về tổng <strong>lớn nhất</strong> của <code>k</code> <span data-keyword="subarray">mảng con</span> không giao nhau trong <code>nums</code>, trong đó mỗi mảng con có độ dài <strong>ít nhất</strong> <code>m</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,-1,3,3,4], k = 2, m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lựa chọn tối ưu là:</p>

<ul>
	<li>Mảng con <code>nums[3..5]</code> có tổng <code>3 + 3 + 4 = 10</code> (độ dài là <code>3 &gt;= m</code>).</li>
	<li>Mảng con <code>nums[0..1]</code> có tổng <code>1 + 2 = 3</code> (độ dài là <code>2 &gt;= m</code>).</li>
</ul>

<p>Tổng cộng là <code>10 + 3 = 13</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-10,3,-1,-2], k = 4, m = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lựa chọn tối ưu là chọn mỗi phần tử làm một mảng con. Kết quả là <code>(-10) + 3 + (-1) + (-2) = -10</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= floor(nums.length / m)</code></li>
	<li><code>1 &lt;= m &lt;= 3</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta chọn $k$ mảng con không giao nhau có độ dài ít nhất $m$ và tối đa hóa tổng của chúng. Tích của $n$, $k$ và $m$ phải nằm trong phạm vi của DP.
>
> Việc hiện tại có đang ở bên trong một đoạn hay không phải được đưa vào trạng thái, nếu không ta không thể đảm bảo giới hạn độ dài.
>
> $f[i][j][0/1]$ xét $i$ phần tử đầu tiên, $j$ đoạn đã hoàn thành và việc ta có đang ở bên trong một đoạn hay không. Ta có thể bỏ qua $i$, hoặc bắt đầu/kéo dài một đoạn. Đáp án là $f[n][k][*]$.

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
