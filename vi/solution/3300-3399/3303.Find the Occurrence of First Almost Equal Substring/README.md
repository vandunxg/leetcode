---
comments: true
difficulty: Hard
rating: 2509
source: Biweekly Contest 140 Q4
tags:
    - String
    - String Matching
    - KMP
---

<!-- problem:start -->

# [3303. Find the Occurrence of First Almost Equal Substring](https://leetcode.com/problems/find-the-occurrence-of-first-almost-equal-substring)

[中文文档](/solution/3300-3399/3303.Find%20the%20Occurrence%20of%20First%20Almost%20Equal%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>pattern</code>.</p>

<p>Một chuỗi <code>x</code> được gọi là <strong>gần bằng</strong> với <code>y</code> nếu có thể thay đổi <strong>nhiều nhất</strong> một ký tự trong <code>x</code> để biến nó thành <em>giống hệt</em> <code>y</code>.</p>

<p>Trả về <strong>nhỏ nhất</strong> trong các <em>chỉ số bắt đầu</em> của một <span data-keyword="substring-nonempty">substring</span> trong <code>s</code> <strong>gần bằng</strong> với <code>pattern</code>. Nếu không tồn tại chỉ số nào như vậy, trả về <code>-1</code>.</p>
Một <strong>substring</strong> là một dãy ký tự <b>liền kề và không rỗng</b> trong một chuỗi.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcdefg&quot;, pattern = &quot;bcdffg&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Substring <code>s[1..6] == &quot;bcdefg&quot;</code> có thể được chuyển thành <code>&quot;bcdffg&quot;</code> bằng cách thay <code>s[4]</code> thành <code>&quot;f&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;ababbababa&quot;, pattern = &quot;bacaba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Substring <code>s[4..9] == &quot;bababa&quot;</code> có thể được chuyển thành <code>&quot;bacaba&quot;</code> bằng cách thay <code>s[6]</code> thành <code>&quot;c&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcd&quot;, pattern = &quot;dba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;dde&quot;, pattern = &quot;d&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt; s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>pattern</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán này nếu có thể thay đổi <strong>nhiều nhất</strong> <code>k</code> ký tự <strong>liên tiếp</strong> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một cửa sổ gần bằng với $\textit{pattern}$ khi khoảng cách Hamming giữa chúng không vượt quá một. So sánh từng ký tự của mọi cửa sổ sẽ tốn $O(|s| \cdot |\textit{pattern}|)$, quá chậm với độ dài lên đến $10^5$.
>
> Tối đa một vị trí không khớp nghĩa là phần khớp tiền tố và phần khớp hậu tố khi ghép lại sẽ bao phủ toàn bộ cửa sổ, chỉ còn nhiều nhất một khoảng trống. Có thể dùng mảng Z xuôi và ngược (hoặc hash chuỗi) để kiểm tra điều này trong thời gian tuyến tính.
>
> Với mỗi vị trí bắt đầu, ta kiểm tra xem tổng độ dài phần khớp tiền tố và phần khớp hậu tố có ít nhất bằng $|\textit{pattern}|-1$ hay không, rồi trả về chỉ số hợp lệ đầu tiên; nếu không có thì trả về $-1$.

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
