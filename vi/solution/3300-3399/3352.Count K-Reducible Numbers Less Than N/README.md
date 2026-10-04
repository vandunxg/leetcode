---
comments: true
difficulty: Hard
rating: 2450
source: Weekly Contest 423 Q4
tags:
    - Math
    - String
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [3352. Count K-Reducible Numbers Less Than N](https://leetcode.com/problems/count-k-reducible-numbers-less-than-n)

[中文文档](/solution/3300-3399/3352.Count%20K-Reducible%20Numbers%20Less%20Than%20N/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>nhị phân</strong> <code>s</code> biểu diễn số <code>n</code> ở dạng nhị phân.</p>

<p>Bạn cũng được cho một số nguyên <code>k</code>.</p>

<p>Một số nguyên <code>x</code> được gọi là <strong>k-reducible</strong> nếu thực hiện phép toán sau <strong>không quá</strong> <code>k</code> lần sẽ đưa nó về 1:</p>

<ul>
    <li>Thay <code>x</code> bằng <strong>số lượng</strong> <span data-keyword="set-bit">bit 1</span> trong biểu diễn nhị phân của nó.</li>
</ul>

<p>Ví dụ, biểu diễn nhị phân của 6 là <code>&quot;110&quot;</code>. Thực hiện phép toán một lần sẽ đưa nó về 2 (vì <code>&quot;110&quot;</code> có hai bit 1). Thực hiện phép toán một lần nữa với 2 (nhị phân là <code>&quot;10&quot;</code>) sẽ đưa nó về 1 (vì <code>&quot;10&quot;</code> có một bit 1).</p>

<p>Trả về một số nguyên biểu thị số lượng số nguyên dương <strong>nhỏ hơn</strong> <code>n</code> là <strong>k-reducible</strong>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;111&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích: </strong></p>

<p><code>n = 7</code>. Các số nguyên 1-reducible nhỏ hơn 7 là 1, 2 và 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1000&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>n = 8</code>. Các số nguyên 2-reducible nhỏ hơn 8 là 1, 2, 3, 4, 5 và 6.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nguyên dương nào nhỏ hơn <code>n = 1</code>, nên đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 800</code></li>
    <li><code>s</code> không có số 0 ở đầu.</li>
    <li><code>s</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
    <li><code>1 &lt;= k &lt;= 5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một phép toán thay $x$ bằng popcount của nó. Ta đếm các giá trị nhỏ hơn $n$ đạt tới $1$ trong không quá $k$ phép toán. Với $|s| \le 800$, đây là bài toán digit DP.
>
> Với $x>1$, tính $k$-reducible chỉ phụ thuộc vào $\operatorname{popcount}(x)$, nên ta tính trước số bước cần thiết để các số $1..800$ đạt tới $1$.
>
> Sau đó, digit DP trên chuỗi nhị phân đếm các số nhỏ hơn $n$ có chính xác $c$ bit 1; một số là hợp lệ khi bản thân $c$ là $(k-1)$-reducible. Vì $k \le 5$, phần tính trước này rất nông.

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
