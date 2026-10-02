---
comments: true
difficulty: Hard
rating: 2666
source: Weekly Contest 182 Q4
tags:
    - String
    - Dynamic Programming
    - String Matching
---

<!-- problem:start -->

# [1397. Find All Good Strings](https://leetcode.com/problems/find-all-good-strings)

[中文文档](/solution/1300-1399/1397.Find%20All%20Good%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code> có độ dài <code>n</code>, cùng chuỗi <code>evil</code>. Hãy trả về <em>số lượng chuỗi <strong>tốt</strong></em>.</p>

<p>Một chuỗi <strong>tốt</strong> có độ dài <code>n</code>, lớn hơn hoặc bằng <code>s1</code> theo thứ tự từ điển, nhỏ hơn hoặc bằng <code>s2</code> theo thứ tự từ điển và không chứa chuỗi <code>evil</code> làm chuỗi con. Vì đáp án có thể rất lớn, hãy trả về phần dư khi chia đáp án cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, s1 = &quot;aa&quot;, s2 = &quot;da&quot;, evil = &quot;b&quot;
<strong>Đầu ra:</strong> 51 
<strong>Giải thích:</strong> Có 25 chuỗi tốt bắt đầu bằng &#39;a&#39;: &quot;aa&quot;,&quot;ac&quot;,&quot;ad&quot;,...,&quot;az&quot;. Tiếp theo có 25 chuỗi tốt bắt đầu bằng &#39;c&#39;: &quot;ca&quot;,&quot;cc&quot;,&quot;cd&quot;,...,&quot;cz&quot;, và cuối cùng có một chuỗi tốt bắt đầu bằng &#39;d&#39;: &quot;da&quot;.&nbsp;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 8, s1 = &quot;leetcode&quot;, s2 = &quot;leetgoes&quot;, evil = &quot;leet&quot;
<strong>Đầu ra:</strong> 0 
<strong>Giải thích:</strong> Tất cả chuỗi lớn hơn hoặc bằng s1 và nhỏ hơn hoặc bằng s2 đều bắt đầu bằng tiền tố &quot;leet&quot;, vì vậy không có chuỗi tốt nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, s1 = &quot;gx&quot;, s2 = &quot;gz&quot;, evil = &quot;x&quot;
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>s1.length == n</code></li>
	<li><code>s2.length == n</code></li>
	<li><code>s1 &lt;= s2</code></li>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>1 &lt;= evil.length &lt;= 50</code></li>
	<li>Tất cả chuỗi chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chuỗi độ dài $n$ nằm trong đoạn $[s_1,s_2]$ và không chứa $\textit{evil}$ làm chuỗi con. Vì $n \le 500$, không thể liệt kê tất cả chuỗi. Digit DP xây dựng chuỗi từ trái sang phải, còn KMP automaton theo dõi độ dài tiền tố của $\textit{evil}$ đã khớp; không được để độ dài khớp đạt $|\textit{evil}|$. Dùng memoization để tính số chuỗi không vượt quá $s_2$ và số chuỗi nhỏ hơn $s_1$, rồi lấy hiệu theo modulo số nguyên tố.

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
