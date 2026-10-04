---
comments: true
difficulty: Hard
rating: 3027
source: Weekly Contest 434 Q4
tags:
    - Bit Manipulation
    - Graph
    - Topological Sort
    - Array
    - String
    - Enumeration
---

<!-- problem:start -->

# [3435. Frequencies of Shortest Supersequences](https://leetcode.com/problems/frequencies-of-shortest-supersequences)

[中文文档](/solution/3400-3499/3435.Frequencies%20of%20Shortest%20Supersequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code>. Hãy tìm tất cả <strong>chuỗi siêu dãy chung ngắn nhất (SCS)</strong> của <code><font face="monospace">words</font></code> không phải là <span data-keyword="permutation-string">hoán vị</span> của nhau.</p>

<p><strong>Chuỗi siêu dãy chung ngắn nhất</strong> là chuỗi có độ dài <strong>nhỏ nhất</strong> chứa mỗi chuỗi trong <code>words</code> dưới dạng <span data-keyword="subsequence-string-nonempty">dãy con</span>.</p>

<p>Hãy trả về mảng số nguyên 2D <code>freqs</code> biểu diễn tất cả các SCS. Mỗi <code>freqs[i]</code> là một mảng có kích thước 26, biểu diễn tần suất của mỗi chữ cái trong bảng chữ cái tiếng Anh viết thường trong một SCS. Bạn có thể trả về các mảng tần suất theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;ab&quot;,&quot;ba&quot;]</span></p>

<p><strong>Đầu ra: </strong>[[1,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],[2,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]]</p>

<p><strong>Giải thích:</strong></p>

<p>Hai SCS là <code>&quot;aba&quot;</code> và <code>&quot;bab&quot;</code>. Kết quả là tần suất các chữ cái của mỗi chuỗi.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;aa&quot;,&quot;ac&quot;]</span></p>

<p><strong>Đầu ra: </strong>[[2,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]]</p>

<p><strong>Giải thích:</strong></p>

<p>Hai SCS là <code>&quot;aac&quot;</code> và <code>&quot;aca&quot;</code>. Vì chúng là hoán vị của nhau nên chỉ giữ lại <code>&quot;aac&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = </span>[&quot;aa&quot;,&quot;bb&quot;,&quot;cc&quot;]</p>

<p><strong>Đầu ra: </strong>[[2,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]]</p>

<p><strong>Giải thích:</strong></p>

<p><code>&quot;aabbcc&quot;</code> và mọi hoán vị của nó đều là các SCS.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= words.length &lt;= 256</code></li>
    <li><code>words[i].length == 2</code></li>
    <li>Tổng tất cả các chuỗi trong <code>words</code> chỉ gồm không quá 16 chữ cái thường khác nhau.</li>
    <li>Tất cả các chuỗi trong <code>words</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi từ có độ dài $2$ và sử dụng nhiều nhất $16$ chữ cái. Tần suất của mỗi chữ cái trong một chuỗi siêu dãy chung ngắn nhất là $1$ hoặc $2$, đồng thời phải thỏa mãn mọi ràng buộc độ dài $2$.
>
> Các chữ cái là các đỉnh, còn các từ là các cạnh có hướng. Các chữ cái nằm trên một chu trình phải xuất hiện hai lần; trong một DAG, xuất hiện một lần là đủ và độ dài được quyết định bởi chuỗi dài nhất.
>
> Với nhiều nhất $16$ đỉnh, ta liệt kê các chữ cái được nhân đôi (một feedback vertex set), kiểm tra phần còn lại có phải là DAG hay không, rồi thu thập các mảng tần suất của tất cả các chuỗi siêu dãy chung ngắn nhất.

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
