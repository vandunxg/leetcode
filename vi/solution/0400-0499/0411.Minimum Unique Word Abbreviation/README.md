---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - String
    - Backtracking
---

<!-- problem:start -->

# [411. Minimum Unique Word Abbreviation 🔒](https://leetcode.com/problems/minimum-unique-word-abbreviation)

[中文文档](/solution/0400-0499/0411.Minimum%20Unique%20Word%20Abbreviation/README.md)

## Mô tả

<!-- description:start -->

<p>Có thể <strong>viết tắt</strong> một chuỗi bằng cách thay thế một số chuỗi con <strong>không liền kề</strong> bằng độ dài của chúng. Ví dụ, chuỗi <code>&quot;substitution&quot;</code> có thể được viết tắt như sau (và còn nhiều cách khác):</p>

<ul>
	<li><code>&quot;s10n&quot;</code> (<code>&quot;s <u>ubstitutio</u> n&quot;</code>)</li>
	<li><code>&quot;sub4u4&quot;</code> (<code>&quot;sub <u>stit</u> u <u>tion</u>&quot;</code>)</li>
	<li><code>&quot;12&quot;</code> (<code>&quot;<u>substitution</u>&quot;</code>)</li>
	<li><code>&quot;su3i1u2on&quot;</code> (<code>&quot;su <u>bst</u> i <u>t</u> u <u>ti</u> on&quot;</code>)</li>
	<li><code>&quot;substitution&quot;</code> (không thay thế chuỗi con nào)</li>
</ul>

<p>Lưu ý, <code>&quot;s55n&quot;</code> (<code>&quot;s <u>ubsti</u> <u>tutio</u> n&quot;</code>) không phải cách viết tắt hợp lệ của <code>&quot;substitution&quot;</code> vì các chuỗi con bị thay thế nằm liền kề nhau.</p>

<p><strong>Độ dài</strong> của một cách viết tắt bằng số chữ cái được giữ lại cộng với số chuỗi con đã được thay thế. Ví dụ, <code>&quot;s10n&quot;</code> có độ dài <code>3</code> (<code>2</code> chữ cái + <code>1</code> chuỗi con), còn <code>&quot;su3i1u2on&quot;</code> có độ dài <code>9</code> (<code>6</code> chữ cái + <code>3</code> chuỗi con).</p>

<p>Cho chuỗi đích <code>target</code> và mảng chuỗi <code>dictionary</code>, hãy trả về <em>một cách <strong>viết tắt</strong> của </em><code>target</code><em> có <strong>độ dài ngắn nhất có thể</strong>, sao cho nó <strong>không phải cách viết tắt</strong> của <strong>bất kỳ</strong> chuỗi nào trong </em><code>dictionary</code><em>. Nếu có nhiều cách viết tắt ngắn nhất, có thể trả về bất kỳ cách nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = &quot;apple&quot;, dictionary = [&quot;blade&quot;]
<strong>Đầu ra:</strong> &quot;a4&quot;
<strong>Giải thích:</strong> Cách viết tắt ngắn nhất của &quot;apple&quot; là &quot;5&quot;, nhưng đây cũng là cách viết tắt của &quot;blade&quot;.
Các cách viết tắt ngắn tiếp theo là &quot;a4&quot; và &quot;4e&quot;. &quot;4e&quot; là cách viết tắt của blade, còn &quot;a4&quot; thì không.
Vì vậy, trả về &quot;a4&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = &quot;apple&quot;, dictionary = [&quot;blade&quot;,&quot;plain&quot;,&quot;amber&quot;]
<strong>Đầu ra:</strong> &quot;1p3&quot;
<strong>Giải thích:</strong> &quot;5&quot; là cách viết tắt của &quot;apple&quot; và cả mọi từ trong dictionary.
&quot;a4&quot; là cách viết tắt của &quot;apple&quot; nhưng cũng là của &quot;amber&quot;.
&quot;4e&quot; là cách viết tắt của &quot;apple&quot; nhưng cũng là của &quot;blade&quot;.
&quot;1p3&quot;, &quot;2p2&quot; và &quot;3l1&quot; là các cách viết tắt ngắn tiếp theo của &quot;apple&quot;.
Vì không cách nào trong số đó là cách viết tắt của từ nào trong dictionary, trả về bất kỳ cách nào cũng đúng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == target.length</code></li>
	<li><code>n == dictionary.length</code></li>
	<li><code>1 &lt;= m &lt;= 21</code></li>
	<li><code>0 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= dictionary[i].length &lt;= 100</code></li>
	<li><code>log<sub>2</sub>(n) + m &lt;= 21</code> if <code>n &gt; 0</code></li>
	<li><code>target</code> và <code>dictionary[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>dictionary</code> không chứa <code>target</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các tab ngôn ngữ của bài này đang để trống trong repository, tức là chưa có thuật toán được cài đặt. Tuy vậy, ta vẫn cần tìm cách viết tắt ngắn nhất của $\textit{target}$ không hợp lệ với bất kỳ từ nào trong dictionary.
>
> $m\le 21$ và $\log_2 n+m\le 21$ cho phép liệt kê các vị trí cần giữ lại. Các đoạn bị thay thế liền kề sẽ gộp thành một số; độ dài bằng số chữ cái được giữ cộng với số đoạn bị thay thế.
>
> Chỉ các từ trong dictionary có cùng độ dài mới có thể xung đột: cách viết tắt là duy nhất nếu không có từ nào trong số đó trùng với $\textit{target}$ ở tất cả vị trí được giữ lại.

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
