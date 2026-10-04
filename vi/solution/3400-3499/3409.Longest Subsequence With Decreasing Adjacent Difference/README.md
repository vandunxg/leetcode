---
comments: true
difficulty: Medium
rating: 2500
source: Biweekly Contest 147 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3409. Longest Subsequence With Decreasing Adjacent Difference](https://leetcode.com/problems/longest-subsequence-with-decreasing-adjacent-difference)

[中文文档](/solution/3400-3499/3409.Longest%20Subsequence%20With%20Decreasing%20Adjacent%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Nhiệm vụ của bạn là tìm độ dài của <span data-keyword="subsequence-array">dãy con</span> <strong>dài nhất</strong> <code>seq</code> của <code>nums</code>, sao cho <strong>hiệu tuyệt đối</strong> giữa các phần tử <em>liên tiếp</em> tạo thành một <strong>dãy số nguyên không tăng</strong>. Nói cách khác, với dãy con <code>seq<sub>0</sub></code>, <code>seq<sub>1</sub></code>, <code>seq<sub>2</sub></code>, ..., <code>seq<sub>m</sub></code> của <code>nums</code>, ta có <code>|seq<sub>1</sub> - seq<sub>0</sub>| &gt;= |seq<sub>2</sub> - seq<sub>1</sub>| &gt;= ... &gt;= |seq<sub>m</sub> - seq<sub>m - 1</sub>|</code>.</p>

<p>Trả về độ dài của dãy con như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [16,6,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Dãy con dài nhất là <code>[16, 6, 3]</code> với các hiệu tuyệt đối giữa các phần tử kề nhau là <code>[10, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,5,3,4,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con dài nhất là <code>[6, 4, 2, 1]</code> với các hiệu tuyệt đối giữa các phần tử kề nhau là <code>[2, 2, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,20,10,19,10,20]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Dãy con dài nhất là <code>[10, 20, 10, 19, 10]</code> với các hiệu tuyệt đối giữa các phần tử kề nhau là <code>[10, 10, 9, 9]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 300</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hiệu tuyệt đối giữa các phần tử kề nhau trong dãy con phải giảm nghiêm ngặt. $n\le 10^4$ loại trừ việc liệt kê các tập con, nhưng các giá trị nằm trong $[1,300]$, nên hiệu lớn nhất là $299$.
>
> Một trạng thái chỉ cần giá trị cuối cùng và hiệu cuối cùng; các chuyển trạng thái có thể chạy trên miền nhỏ đó.
>
> Đặt $f[v][d]$ là độ dài dãy con dài nhất kết thúc bằng giá trị $v$ và có hiệu cuối cùng là $d$. Khi thêm $x$, ta liệt kê một giá trị trước đó $y$ và một hiệu lớn hơn $d>|x-y|$, rồi cập nhật $f[x][|x-y|]$ từ $f[y][d]$.

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
