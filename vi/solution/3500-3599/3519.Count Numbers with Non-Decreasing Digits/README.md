---
comments: true
difficulty: Hard
rating: 2246
source: Weekly Contest 445 Q4
tags:
    - Math
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3519. Count Numbers with Non-Decreasing Digits](https://leetcode.com/problems/count-numbers-with-non-decreasing-digits)

[中文文档](/solution/3500-3599/3519.Count%20Numbers%20with%20Non-Decreasing%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>l</code> và <code>r</code> được biểu diễn dưới dạng chuỗi, cùng một số nguyên <code>b</code>. Hãy trả về số lượng số nguyên trong đoạn bao gồm <code>[l, r]</code> có các chữ số theo thứ tự <strong>không giảm</strong> khi được biểu diễn trong cơ số <code>b</code>.</p>

<p>Một số nguyên được coi là có các chữ số theo thứ tự <strong>không giảm</strong> nếu khi đọc từ trái sang phải (từ chữ số có nghĩa lớn nhất đến chữ số có nghĩa nhỏ nhất), mỗi chữ số lớn hơn hoặc bằng chữ số đứng trước nó.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = &quot;23&quot;, r = &quot;28&quot;, b = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các số từ 23 đến 28 trong cơ số 8 là: 27, 30, 31, 32, 33 và 34.</li>
	<li>Trong số này, 27, 33 và 34 có các chữ số theo thứ tự không giảm. Do đó, kết quả là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = &quot;2&quot;, r = &quot;7&quot;, b = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các số từ 2 đến 7 trong cơ số 2 là: 10, 11, 100, 101, 110 và 111.</li>
	<li>Trong số này, 11 và 111 có các chữ số theo thứ tự không giảm. Do đó, kết quả là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code><font face="monospace">1 &lt;= l.length &lt;= r.length &lt;= 100</font></code></li>
	<li><code>2 &lt;= b &lt;= 10</code></li>
	<li><code>l</code> và <code>r</code> chỉ gồm các chữ số.</li>
	<li>Giá trị được biểu diễn bởi <code>l</code> nhỏ hơn hoặc bằng giá trị được biểu diễn bởi <code>r</code>.</li>
	<li><code>l</code> và <code>r</code> không chứa các số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $l$ và $r$ có tối đa $100$ chữ số, nên không thể duyệt qua toàn bộ đoạn. Ta đếm các số nguyên có các chữ số trong cơ số $b$ theo thứ tự không giảm bằng $f(r) - f(l-1)$.
>
> $f(x)$ là một digit DP: điền từ chữ số cao xuống, không bao giờ giảm, đồng thời theo dõi cờ giới hạn trên. Kết quả được lấy modulo $10^9+7$.

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
