---
comments: true
difficulty: Hard
rating: 2473
source: Biweekly Contest 151 Q4
tags:
    - Array
    - Math
    - Combinatorics
    - Enumeration
---

<!-- problem:start -->

# [3470. Permutations IV](https://leetcode.com/problems/permutations-iv)

[中文文档](/solution/3400-3499/3470.Permutations%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>k</code>, một <strong>hoán vị xen kẽ</strong> là một hoán vị của <code>n</code> số nguyên dương đầu tiên sao cho không có <strong>hai</strong> phần tử kề nhau nào đều là số lẻ hoặc đều là số chẵn.</p>

<p>Hãy trả về <strong>hoán vị xen kẽ</strong> thứ <strong>k</strong> theo <em>thứ tự từ điển</em>. Nếu có ít hơn <code>k</code> <strong>hoán vị xen kẽ</strong> hợp lệ, hãy trả về một danh sách rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,4,1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các hoán vị xen kẽ của <code>[1, 2, 3, 4]</code> theo thứ tự từ điển là:</p>

<ol>
	<li><code>[1, 2, 3, 4]</code></li>
	<li><code>[1, 4, 3, 2]</code></li>
	<li><code>[2, 1, 4, 3]</code></li>
	<li><code>[2, 3, 4, 1]</code></li>
	<li><code>[3, 2, 1, 4]</code></li>
	<li><code>[3, 4, 1, 2]</code> &larr; hoán vị thứ 6</li>
	<li><code>[4, 1, 2, 3]</code></li>
	<li><code>[4, 3, 2, 1]</code></li>
</ol>

<p>Vì <code>k = 6</code>, ta trả về <code>[3, 4, 1, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các hoán vị xen kẽ của <code>[1, 2, 3]</code> theo thứ tự từ điển là:</p>

<ol>
	<li><code>[1, 2, 3]</code></li>
	<li><code>[3, 2, 1]</code> &larr; hoán vị thứ 2</li>
</ol>

<p>Vì <code>k = 2</code>, ta trả về <code>[3, 2, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các hoán vị xen kẽ của <code>[1, 2]</code> theo thứ tự từ điển là:</p>

<ol>
	<li><code>[1, 2]</code></li>
	<li><code>[2, 1]</code></li>
</ol>

<p>Chỉ có 2 hoán vị xen kẽ, nhưng <code>k = 3</code> nằm ngoài phạm vi. Do đó, ta trả về một danh sách rỗng <code>[]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm hoán vị thứ $k$ của $1..n$ có tính chẵn lẻ xen kẽ. Vì $n$ quá lớn, không thể liệt kê tất cả hoán vị.
>
> Khi tính chẵn lẻ của vị trí đầu tiên được cố định, phần còn lại được quyết định theo đó, và số lượng hoán vị là tích của các giai thừa cùng số lượng phần tử lẻ/chẵn còn lại.
>
> Ta điền từ trái sang phải: thử từng ứng viên, dùng số lượng hậu tố xen kẽ để xem $k$ có thuộc nhóm đó không, trừ đi số lượng tương ứng rồi tiếp tục. Nếu $k$ quá lớn, trả về danh sách rỗng.

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
