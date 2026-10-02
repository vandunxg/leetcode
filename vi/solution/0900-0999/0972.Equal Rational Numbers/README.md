---
comments: true
difficulty: Hard
tags:
    - Math
    - String
---

<!-- problem:start -->

# [972. Equal Rational Numbers](https://leetcode.com/problems/equal-rational-numbers)

[中文文档](/solution/0900-0999/0972.Equal%20Rational%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code>, mỗi chuỗi biểu diễn một số hữu tỉ không âm. Trả về <code>true</code> khi và chỉ khi chúng biểu diễn cùng một số. Chuỗi có thể dùng dấu ngoặc đơn để biểu thị phần tuần hoàn của số hữu tỉ.</p>

<p>Một <strong>số hữu tỉ</strong> có thể gồm tối đa ba phần: <code>&lt;IntegerPart&gt;</code>, <code>&lt;NonRepeatingPart&gt;</code> và <code>&lt;RepeatingPart&gt;</code>. Số đó được biểu diễn theo một trong ba dạng sau:</p>

<ul>
	<li><code>&lt;IntegerPart&gt;</code>

    <ul>
    	<li>Ví dụ, <code>12</code>, <code>0</code> và <code>123</code>.</li>
    </ul>
    </li>
    <li><code>&lt;IntegerPart&gt;<strong>&lt;.&gt;</strong>&lt;NonRepeatingPart&gt;</code>
    <ul>
    	<li>Ví dụ, <code>0.5</code>, <code>1.</code>, <code>2.12</code> và <code>123.0001</code>.</li>
    </ul>
    </li>
    <li><code>&lt;IntegerPart&gt;<strong>&lt;.&gt;</strong>&lt;NonRepeatingPart&gt;<strong>&lt;(&gt;</strong>&lt;RepeatingPart&gt;<strong>&lt;)&gt;</strong></code>
    <ul>
    	<li>Ví dụ, <code>0.1(6)</code>, <code>1.(9)</code>, <code>123.00(1212)</code>.</li>
    </ul>
    </li>

</ul>

<p>Theo quy ước, phần tuần hoàn trong khai triển thập phân được đặt trong một cặp dấu ngoặc đơn. Ví dụ:</p>

<ul>
	<li><code>1/6 = 0.16666666... = 0.1(6) = 0.1666(6) = 0.166(66)</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0.(52)&quot;, t = &quot;0.5(25)&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Vì &quot;0.(52)&quot; biểu diễn 0.52525252..., còn &quot;0.5(25)&quot; biểu diễn 0.52525252525....., hai chuỗi biểu diễn cùng một số.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0.1666(6)&quot;, t = &quot;0.166(66)&quot;
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0.9(9)&quot;, t = &quot;1.&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> &quot;0.9(9)&quot; biểu diễn số 0.999999999... tuần hoàn vô hạn, bằng 1.  [<a href="https://en.wikipedia.org/wiki/0.999..." target="_blank">Xem giải thích tại liên kết này.</a>]
&quot;1.&quot; biểu diễn số 1, với dạng hợp lệ: (IntegerPart) = &quot;1&quot; và (NonRepeatingPart) = &quot;&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Mỗi phần chỉ gồm các chữ số.</li>
	<li><code>&lt;IntegerPart&gt;</code> không có số 0 ở đầu (ngoại trừ trường hợp chính nó là số 0).</li>
	<li><code>1 &lt;= &lt;IntegerPart&gt;.length &lt;= 4</code></li>
	<li><code>0 &lt;= &lt;NonRepeatingPart&gt;.length &lt;= 4</code></li>
	<li><code>1 &lt;= &lt;RepeatingPart&gt;.length &lt;= 4</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai số thập phân có phần tuần hoàn có thể trông khác nhau nhưng biểu diễn cùng một số hữu tỉ, chẳng hạn $0.9(9)=1$. Mỗi phần dài tối đa $4$, nên có thể chuyển đổi chính xác sang phân số: biểu diễn phần nguyên, phần không tuần hoàn và phần tuần hoàn dưới dạng tử số, mẫu số; rút gọn rồi so sánh, kể cả trường hợp chữ số $9$ tuần hoàn làm tăng phần nguyên.

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
