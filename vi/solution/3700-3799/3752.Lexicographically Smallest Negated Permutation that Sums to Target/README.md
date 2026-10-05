---
comments: true
difficulty: Medium
rating: 1827
source: Biweekly Contest 170 Q3
tags:
    - Greedy
    - Array
    - Math
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [3752. Lexicographically Smallest Negated Permutation that Sums to Target](https://leetcode.com/problems/lexicographically-smallest-negated-permutation-that-sums-to-target)

[中文文档](/solution/3700-3799/3752.Lexicographically%20Smallest%20Negated%20Permutation%20that%20Sums%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code> và một số nguyên <code>target</code>.</p>

<p>Hãy trả về mảng số nguyên có kích thước <code>n</code> <strong><span data-keyword="lexicographically-smaller-array">nhỏ nhất theo thứ tự từ điển</span></strong> sao cho:</p>

<ul>
	<li><strong>Tổng</strong> các phần tử bằng <code>target</code>.</li>
	<li><strong>Giá trị tuyệt đối</strong> của các phần tử tạo thành một <strong>hoán vị</strong> có kích thước <code>n</code>.</li>
</ul>

<p>Nếu không tồn tại mảng phù hợp, hãy trả về một mảng rỗng.</p>

<p>Một <strong>hoán vị</strong> có kích thước <code>n</code> là một cách sắp xếp lại các số nguyên <code>1, 2, ..., n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, target = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-3,1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng có tổng bằng 0 và có giá trị tuyệt đối tạo thành một hoán vị có kích thước 3 là:</p>

<ul>
	<li><code>[-3, 1, 2]</code></li>
	<li><code>[-3, 2, 1]</code></li>
	<li><code>[-2, -1, 3]</code></li>
	<li><code>[-2, 3, -1]</code></li>
	<li><code>[-1, -2, 3]</code></li>
	<li><code>[-1, 3, -2]</code></li>
	<li><code>[1, -3, 2]</code></li>
	<li><code>[1, 2, -3]</code></li>
	<li><code>[2, -3, 1]</code></li>
	<li><code>[2, 1, -3]</code></li>
	<li><code>[3, -2, -1]</code></li>
	<li><code>[3, -1, -2]</code></li>
</ul>

<p>Mảng nhỏ nhất theo thứ tự từ điển là <code>[-3, 1, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, target = 10000000000</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có mảng nào có tổng bằng <span class="example-io">10000000000 và giá trị tuyệt đối tạo thành một hoán vị có kích thước 1. Vì vậy, đáp án là <code>[]</code>.</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>10</sup> &lt;= target &lt;= 10<sup>10</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị tuyệt đối phải là một hoán vị của $1\ldots n$ có tổng bằng $\textit{target}$. Tổng khi tất cả đều dương là $S=n(n+1)/2$; đổi dấu $x$ làm tổng giảm $2x$, vì vậy $S-\textit{target}$ phải là một số chẵn không âm. Đổi dấu từ lớn đến nhỏ sẽ giữ các số dương nhỏ nhất ở phía trước và tạo ra mảng nhỏ nhất theo thứ tự từ điển.

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
