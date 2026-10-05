---
comments: true
difficulty: Hard
tags:
    - String
    - Binary Search
    - Suffix Array
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3735. Lexicographically Smallest String After Reverse II 🔒](https://leetcode.com/problems/lexicographically-smallest-string-after-reverse-ii)

[中文文档](/solution/3700-3799/3735.Lexicographically%20Smallest%20String%20After%20Reverse%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> có độ dài <code>n</code>, chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn phải thực hiện <strong>chính xác</strong> một thao tác bằng cách chọn một số nguyên <code>k</code> sao cho <code>1 &lt;= k &lt;= n</code> và thực hiện một trong hai việc sau:</p>

<ul>
	<li>đảo ngược <strong>đầu tiên</strong> <code>k</code> ký tự của <code>s</code>, hoặc</li>
	<li>đảo ngược <strong>cuối cùng</strong> <code>k</code> ký tự của <code>s</code>.</li>
</ul>

<p>Trả về chuỗi <strong><span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span></strong> có thể thu được sau khi thực hiện <strong>chính xác</strong> một thao tác như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;dcab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;acdb&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>k = 3</code>, đảo ngược 3 ký tự đầu tiên.</li>
	<li>Đảo ngược <code>&quot;dca&quot;</code> thành <code>&quot;acd&quot;</code>, thu được chuỗi <code>s = &quot;acdb&quot;</code>, đây là chuỗi nhỏ nhất theo thứ tự từ điển có thể đạt được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aabb&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>k = 3</code>, đảo ngược 3 ký tự cuối cùng.</li>
	<li>Đảo ngược <code>&quot;bba&quot;</code> thành <code>&quot;abb&quot;</code>, nên chuỗi kết quả là <code>&quot;aabb&quot;</code>, đây là chuỗi nhỏ nhất theo thứ tự từ điển có thể đạt được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zxy&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;xzy&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>k = 2</code>, đảo ngược 2 ký tự đầu tiên.</li>
	<li>Đảo ngược <code>&quot;zx&quot;</code> thành <code>&quot;xz&quot;</code>, nên chuỗi kết quả là <code>&quot;xzy&quot;</code>, đây là chuỗi nhỏ nhất theo thứ tự từ điển có thể đạt được.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tương tự bài toán đảo ngược tiền tố hoặc hậu tố đúng một lần, ta thực hiện chính xác một thao tác và $k$ chỉ có $n$ giá trị. Với mỗi $k$, ta so sánh việc đảo ngược $k$ ký tự đầu tiên với việc đảo ngược $k$ ký tự cuối cùng, rồi giữ lại chuỗi nhỏ nhất theo thứ tự từ điển.

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
