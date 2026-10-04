---
comments: true
difficulty: Medium
rating: 2093
source: Weekly Contest 434 Q3
tags:
    - Greedy
    - Array
    - Hash Table
    - Dynamic Programming
    - Enumeration
    - Prefix Sum
---

<!-- problem:start -->

# [3434. Maximum Frequency After Subarray Operation](https://leetcode.com/problems/maximum-frequency-after-subarray-operation)

[中文文档](/solution/3400-3499/3434.Maximum%20Frequency%20After%20Subarray%20Operation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> có độ dài <code>n</code>. Đồng thời, cho một số nguyên <code>k</code>.</p>

<p>Bạn thực hiện thao tác sau trên <code>nums</code> <strong>một lần</strong>:</p>

<ul>
    <li>Chọn một <span data-keyword="subarray-nonempty">mảng con</span> <code>nums[i..j]</code> sao cho <code>0 &lt;= i &lt;= j &lt;= n - 1</code>.</li>
    <li>Chọn một số nguyên <code>x</code> và cộng <code>x</code> vào <strong>tất cả</strong> các phần tử trong <code>nums[i..j]</code>.</li>
</ul>

<p>Hãy tìm <strong>tần suất lớn nhất</strong> của giá trị <code>k</code> sau thao tác trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5,6], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi cộng -5 vào <code>nums[2..5]</code>, 1 có tần suất là 2 trong <code>[1, 2, -2, -1, 0, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,2,3,4,5,5,4,3,2,2], k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi cộng 8 vào <code>nums[1..9]</code>, 10 có tần suất là 4 trong <code>[10, 10, 11, 12, 13, 13, 12, 11, 10, 10]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 50</code></li>
    <li><code>1 &lt;= k &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác sẽ biến một mảng con thành $k$; mục tiêu là tối đa hóa tần suất của $k$ sau thao tác. $n\le 10^5$ nhưng các giá trị không vượt quá $50$.
>
> Tần suất mới bằng số lượng $k$ ban đầu cộng với số ô khác $k$ trong mảng con được biến thành $k$. Đây là bài toán Kadane trên cách mã hóa $+1/-1$.
>
> Với mỗi giá trị ban đầu $x\neq k$, ta chạy maximum subarray trên $+1$ cho $x$ và $-1$ cho $k$, sau đó cộng với số lượng $k$ trên toàn mảng. Miền giá trị nhỏ khiến độ phức tạp $O(50n)$ là chấp nhận được.

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
