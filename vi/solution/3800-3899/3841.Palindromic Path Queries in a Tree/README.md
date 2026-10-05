---
comments: true
difficulty: Hard
rating: 2384
source: Biweekly Contest 176 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Segment Tree
    - Array
    - String
    - Divide and Conquer
---

<!-- problem:start -->

# [3841. Palindromic Path Queries in a Tree](https://leetcode.com/problems/palindromic-path-queries-in-a-tree)

[中文文档](/solution/3800-3899/3841.Palindromic%20Path%20Queries%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một cây vô hướng gồm <code>n</code> đỉnh, được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh vô hướng nối giữa các đỉnh <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một chuỗi <code>s</code> có độ dài <code>n</code>, chỉ gồm các chữ cái tiếng Anh viết thường, trong đó <code>s[i]</code> là ký tự được gán cho đỉnh <code>i</code>.</p>

<p>Bạn cũng được cho một mảng chuỗi <code>queries</code>, trong đó mỗi <code>queries[i]</code> thuộc một trong hai dạng sau:</p>

<ul>
	<li><code>&quot;update u<sub>i</sub> c&quot;</code>: Thay đổi ký tự tại đỉnh <code>u<sub>i</sub></code> thành <code>c</code>. Cụ thể, cập nhật <code>s[u<sub>i</sub>] = c</code>.</li>
	<li><code>&quot;query u<sub>i</sub> v<sub>i</sub>&quot;</code>: Xác định xem chuỗi tạo bởi các ký tự trên đường đi <strong>duy nhất</strong> từ <code>u<sub>i</sub></code> đến <code>v<sub>i</sub></code> (bao gồm cả hai đầu mút) có thể được <strong>sắp xếp lại</strong> thành một <strong><span data-keyword="palindrome-string">palindrome</span></strong> hay không.</li>
</ul>

<p>Trả về một mảng boolean <code>answer</code>, trong đó <code>answer[j]</code> là <code>true</code> nếu truy vấn <code>j<sup>th</sup></code> có dạng <code>&quot;query u<sub>i</sub> v<sub>i</sub>&quot;​​​​​​​</code> có thể được sắp xếp lại thành một <strong>palindrome</strong>, và là <code>false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], s = &quot;aac&quot;, queries = [&quot;query 0 2&quot;,&quot;update 1 b&quot;,&quot;query 0 2&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,false]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>&quot;query 0 2&quot;</code>: Đường đi <code>0 &rarr; 1 &rarr; 2</code> tạo thành <code>&quot;aac&quot;</code>, có thể được sắp xếp lại thành <code>&quot;aca&quot;</code>, là một palindrome. Do đó, <code>answer[0] = true</code>.</li>
	<li><code>&quot;update 1 b&quot;</code>: Cập nhật đỉnh 1 thành <code>&#39;b&#39;</code>, khi đó <code>s = &quot;abc&quot;</code>.</li>
	<li><code>&quot;query 0 2&quot;</code>: Các ký tự trên đường đi là <code>&quot;abc&quot;</code>, không thể được sắp xếp lại thành một palindrome. Do đó, <code>answer[1] = false</code>.</li>
</ul>

<p>Vì vậy, <code>answer = [true, false]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[0,2],[0,3]], s = &quot;abca&quot;, queries = [&quot;query 1 2&quot;,&quot;update 0 b&quot;,&quot;query 2 3&quot;,&quot;update 3 a&quot;,&quot;query 1 3&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[false,false,true]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>&quot;query 1 2&quot;</code>: Đường đi <code>1 &rarr; 0 &rarr; 2</code> tạo thành <code>&quot;bac&quot;</code>, không thể được sắp xếp lại thành một palindrome. Do đó, <code>answer[0] = false</code>.</li>
	<li><code>&quot;update 0 b&quot;</code>: Cập nhật đỉnh 0 thành <code>&#39;b&#39;</code>, khi đó <code>s = &quot;bbca&quot;</code>.</li>
	<li><code>&quot;query 2 3&quot;</code>: Đường đi <code>2 &rarr; 0 &rarr; 3</code> tạo thành <code>&quot;cba&quot;</code>, không thể được sắp xếp lại thành một palindrome. Do đó, <code>answer[1] = false</code>.</li>
	<li><code>&quot;update 3 a&quot;</code>: Cập nhật đỉnh 3 thành <code>&#39;a&#39;</code>, <code>s = &quot;bbca&quot;</code>.</li>
	<li><code>&quot;query 1 3&quot;</code>: Đường đi <code>1 &rarr; 0 &rarr; 3</code> tạo thành <code>&quot;bba&quot;</code>, có thể được sắp xếp lại thành <code>&quot;bab&quot;</code>, là một palindrome. Do đó, <code>answer[2] = true</code>.</li>
</ul>

<p>Vì vậy, <code>answer = [false, false, true]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code>​​​​​​​
	<ul>
		<li><code>queries[i] = &quot;update u<sub>i</sub> c&quot;</code> hoặc</li>
		<li><code>queries[i] = &quot;query u<sub>i</sub> v<sub>i</sub>&quot;</code></li>
		<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
		<li><code>c</code> là một chữ cái tiếng Anh viết thường.</li>
	</ul>
	</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một đường đi có thể được sắp xếp lại thành palindrome khi và chỉ khi có nhiều nhất một ký tự xuất hiện với số lần lẻ. Với $n,q \le 5 \times 10^4$, không thể duyệt từng đường đi.
>
> Tính chẵn lẻ của số lần xuất hiện các chữ cái được biểu diễn bằng một mask $26$-bit. Mask của đường đi là XOR của hai prefix hướng về gốc, trong đó phần LCA được triệt tiêu.
>
> Việc cập nhật ký tự của một đỉnh làm thay đổi các mask hướng về gốc theo một cấu trúc có quy luật, có thể duy trì bằng cấu trúc hiệu trên cây hoặc cấu trúc Euler tour.
>
> Một truy vấn lấy LCA rồi kiểm tra mask của đường đi có nhiều nhất một bit được bật hay không; một lần cập nhật sẽ ghi lại ký tự và các mask.

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
