---
comments: true
difficulty: Hard
rating: 2386
source: Weekly Contest 436 Q3
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3448. Count Substrings Divisible By Last Digit](https://leetcode.com/problems/count-substrings-divisible-by-last-digit)

[中文文档](/solution/3400-3499/3448.Count%20Substrings%20Divisible%20By%20Last%20Digit/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ số.</p>

<p>Hãy trả về <strong>số lượng</strong> <span data-keyword="substring-nonempty">chuỗi con</span> của <code>s</code> <strong>chia hết</strong> cho chữ số <strong>khác 0</strong> ở cuối chuỗi con đó.</p>

<p><strong>Lưu ý</strong>: Chuỗi con có thể bắt đầu bằng các số 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;12936&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con <code>&quot;29&quot;</code>, <code>&quot;129&quot;</code>, <code>&quot;293&quot;</code> và <code>&quot;2936&quot;</code> không chia hết cho chữ số cuối của chúng. Có tổng cộng 15 chuỗi con, nên đáp án là <code>15 - 4 = 11</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;5701283&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">18</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con <code>&quot;01&quot;</code>, <code>&quot;12&quot;</code>, <code>&quot;701&quot;</code>, <code>&quot;012&quot;</code>, <code>&quot;128&quot;</code>, <code>&quot;5701&quot;</code>, <code>&quot;7012&quot;</code>, <code>&quot;0128&quot;</code>, <code>&quot;57012&quot;</code>, <code>&quot;70128&quot;</code>, <code>&quot;570128&quot;</code> và <code>&quot;701283&quot;</code> đều chia hết cho chữ số cuối của chúng. Ngoài ra, mọi chuỗi con chỉ gồm một chữ số khác 0 đều chia hết cho chính chữ số đó. Vì có 6 chữ số như vậy, đáp án là <code>12 + 6 = 18</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1010101010&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ các chuỗi con kết thúc bằng chữ số <code>&#39;1&#39;</code> mới chia hết cho chữ số cuối của chúng. Có 25 chuỗi con như vậy.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con, khi đọc như một số thập phân, phải chia hết cho chữ số cuối của nó. Với $n\le 10^5$, ta không thể kiểm tra mọi chuỗi con.
>
> Chữ số cuối $d$ nằm trong $[1,9]$. Điều kiện chia hết tương đương với giá trị bằng $0$ modulo $d$. Khi chữ số hiện tại là chữ số cuối, ta lưu số lượng các tiền tố có phần dư $r$.
>
> Nếu phần dư của tiền tố là $p$, một vị trí bắt đầu $l$ là hợp lệ khi $p\equiv 10^{i-l+1}\cdot(\textit{prefix}_{l-1})\pmod d$. Gom nhóm theo chữ số cuối, ta thu được một digit-DP / phép đếm tiền tố tuyến tính. Chữ số cuối bằng $0$ không đóng góp gì.

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
