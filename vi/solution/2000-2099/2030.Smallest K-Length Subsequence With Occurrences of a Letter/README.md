---
comments: true
difficulty: Hard
rating: 2561
source: Weekly Contest 261 Q4
tags:
    - Stack
    - Greedy
    - String
    - Monotonic Stack
---

<!-- problem:start -->

# [2030. Smallest K-Length Subsequence With Occurrences of a Letter](https://leetcode.com/problems/smallest-k-length-subsequence-with-occurrences-of-a-letter)

[中文文档](/solution/2000-2099/2030.Smallest%20K-Length%20Subsequence%20With%20Occurrences%20of%20a%20Letter/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, một số nguyên <code>k</code>, một ký tự <code>letter</code> và một số nguyên <code>repetition</code>.</p>

<p>Hãy trả về <em><strong>phân dãy con nhỏ nhất theo thứ tự từ điển</strong> của</em> <code>s</code><em> có độ dài</em> <code>k</code> <em>trong đó ký tự</em> <code>letter</code> <em>xuất hiện <strong>ít nhất</strong></em> <code>repetition</code> <em>lần</em>. Các test được tạo sao cho <code>letter</code> xuất hiện trong <code>s</code> <strong>ít nhất</strong> <code>repetition</code> lần.</p>

<p><strong>Phân dãy con</strong> là một chuỗi có thể được tạo từ một chuỗi khác bằng cách xóa một số hoặc không xóa ký tự nào mà không thay đổi thứ tự của các ký tự còn lại.</p>

<p>Một chuỗi <code>a</code> được gọi là <strong>nhỏ hơn theo thứ tự từ điển</strong> chuỗi <code>b</code> nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, ký tự của <code>a</code> xuất hiện sớm hơn trong bảng chữ cái so với ký tự tương ứng của <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leet&quot;, k = 3, letter = &quot;e&quot;, repetition = 1
<strong>Đầu ra:</strong> &quot;eet&quot;
<strong>Giải thích:</strong> Có bốn phân dãy con độ dài 3 trong đó ký tự &#39;e&#39; xuất hiện ít nhất 1 lần:
- &quot;lee&quot; (từ &quot;<strong><u>lee</u></strong>t&quot;)
- &quot;let&quot; (từ &quot;<strong><u>le</u></strong>e<u><strong>t</strong></u>&quot;)
- &quot;let&quot; (từ &quot;<u><strong>l</strong></u>e<u><strong>et</strong></u>&quot;)
- &quot;eet&quot; (từ &quot;l<u><strong>eet</strong></u>&quot;)
Phân dãy con nhỏ nhất theo thứ tự từ điển trong số đó là &quot;eet&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="example-2" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2030.Smallest%20K-Length%20Subsequence%20With%20Occurrences%20of%20a%20Letter/images/smallest-k-length-subsequence.png" style="width: 339px; height: 67px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;, k = 4, letter = &quot;e&quot;, repetition = 2
<strong>Đầu ra:</strong> &quot;ecde&quot;
<strong>Giải thích:</strong> &quot;ecde&quot; là phân dãy con nhỏ nhất theo thứ tự từ điển có độ dài 4, trong đó ký tự &quot;e&quot; xuất hiện ít nhất 2 lần.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bb&quot;, k = 2, letter = &quot;b&quot;, repetition = 2
<strong>Đầu ra:</strong> &quot;bb&quot;
<strong>Giải thích:</strong> &quot;bb&quot; là phân dãy con duy nhất có độ dài 2, trong đó ký tự &quot;b&quot; xuất hiện ít nhất 2 lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= repetition &lt;= k &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>letter</code> là một chữ cái tiếng Anh viết thường và xuất hiện trong <code>s</code> ít nhất <code>repetition</code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 5 \times 10^4$, không thể liệt kê tất cả các phân dãy con độ dài $k$. Một stack đơn điệu cho phép tạo phân dãy con nhỏ nhất theo thứ tự từ điển, nhưng ta vẫn phải đảm bảo độ dài bằng $k$ và có ít nhất $repetition$ lần xuất hiện của $letter$.
>
> Chỉ được phép pop khi các ký tự còn lại vẫn có thể lấp đầy $k$ vị trí, đồng thời các ký tự đã chọn cộng với số ký tự $letter$ còn lại vẫn đáp ứng điều kiện $repetition$. Ta theo dõi số lượng letter trong hậu tố và số lượng ký tự trong stack.
>
> Duyệt từ trái sang phải, sau đó lấy $k$ ký tự đầu tiên trong stack. Các tab code đều trống; phần lập luận dưới đây dựa trên stack có các ràng buộc này.

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
