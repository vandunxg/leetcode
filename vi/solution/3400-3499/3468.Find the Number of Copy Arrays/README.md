---
comments: true
difficulty: Medium
rating: 1544
source: Biweekly Contest 151 Q2
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3468. Find the Number of Copy Arrays](https://leetcode.com/problems/find-the-number-of-copy-arrays)

[中文文档](/solution/3400-3499/3468.Find%20the%20Number%20of%20Copy%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>original</code> có độ dài <code>n</code> và một mảng 2D <code>bounds</code> có độ dài <code>n x 2</code>, trong đó <code>bounds[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>.</p>

<p>Bạn cần tìm số lượng mảng <strong>khả dĩ</strong> <code>copy</code> có độ dài <code>n</code> sao cho:</p>

<ol>
	<li><code>(copy[i] - copy[i - 1]) == (original[i] - original[i - 1])</code> với <code>1 &lt;= i &lt;= n - 1</code>.</li>
	<li><code>u<sub>i</sub> &lt;= copy[i] &lt;= v<sub>i</sub></code> với <code>0 &lt;= i &lt;= n - 1</code>.</li>
</ol>

<p>Trả về số lượng mảng như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">original = [1,2,3,4], bounds = [[1,2],[2,3],[3,4],[4,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng khả dĩ là:</p>

<ul>
	<li><code>[1, 2, 3, 4]</code></li>
	<li><code>[2, 3, 4, 5]</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">original = [1,2,3,4], bounds = [[1,10],[2,9],[3,8],[4,7]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng khả dĩ là:</p>

<ul>
	<li><code>[1, 2, 3, 4]</code></li>
	<li><code>[2, 3, 4, 5]</code></li>
	<li><code>[3, 4, 5, 6]</code></li>
	<li><code>[4, 5, 6, 7]</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">original = [1,2,1,2], bounds = [[1,1],[2,3],[3,3],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có mảng nào thỏa mãn.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == original.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= original[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>bounds.length == n</code></li>
	<li><code>bounds[i].length == 2</code></li>
	<li><code>1 &lt;= bounds[i][0] &lt;= bounds[i][1] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng copy phải nằm trong các cận tương ứng với từng chỉ số và giữ nguyên các hiệu giữa hai phần tử liên tiếp như mảng original. Khi các hiệu đã cố định, toàn bộ mảng được xác định bởi phần tử đầu tiên.
>
> Nếu phần tử đầu tiên là $x$, phần tử ở chỉ số $i$ là $x+\textit{pref}[i]$ và phải nằm trong $[\textit{bounds}[i][0],\textit{bounds}[i][1]]$. Như vậy, ta có một tập các bất đẳng thức đối với $x$.
>
> Lấy giao các khoảng đó; số điểm nguyên trong giao là số lượng mảng copy, hoặc $0$ nếu giao rỗng.

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
