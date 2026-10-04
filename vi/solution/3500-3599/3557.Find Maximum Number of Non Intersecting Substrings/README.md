---
comments: true
difficulty: Medium
rating: 1719
source: Biweekly Contest 157 Q2
tags:
    - Greedy
    - Hash Table
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3557. Find Maximum Number of Non Intersecting Substrings](https://leetcode.com/problems/find-maximum-number-of-non-intersecting-substrings)

[中文文档](/solution/3500-3599/3557.Find%20Maximum%20Number%20of%20Non%20Intersecting%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code>.</p>

<p>Trả về số lượng <strong>tối đa</strong> các <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong> không giao nhau của word, có độ dài <strong>ít nhất</strong> bốn ký tự và bắt đầu, kết thúc bằng cùng một chữ cái.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;abcdeafdef&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai chuỗi con đó là <code>&quot;abcdea&quot;</code> và <code>&quot;fdef&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;bcdaaaab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con duy nhất là <code>&quot;aaaa&quot;</code>. Lưu ý rằng ta <strong>cũng không thể</strong> chọn <code>&quot;bcdaaaab&quot;</code> vì nó giao với chuỗi con còn lại.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= word.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con phải bắt đầu và kết thúc bằng cùng một chữ cái, đồng thời có độ dài ít nhất $4$; các chuỗi được chọn phải không giao nhau. Với $n \le 2 \cdot 10^5$, không thể liệt kê tất cả các đoạn.
>
> Duyệt từ trái sang phải. Ghi nhớ vị trí bắt đầu chưa được sử dụng gần nhất của mỗi chữ cái; khi chỉ số hiện tại cách vị trí bắt đầu đó ít nhất $3$, chọn đoạn này và xóa vị trí bắt đầu. Việc kết thúc một đoạn ngắn sớm không bao giờ cản trở lựa chọn về sau, nên số lượng thu được là tối đa.

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
