---
comments: true
difficulty: Hard
rating: 2502
source: Weekly Contest 441 Q4
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [3490. Count Beautiful Numbers](https://leetcode.com/problems/count-beautiful-numbers)

[中文文档](/solution/3400-3499/3490.Count%20Beautiful%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên dương, <code><font face="monospace">l</font></code> và <code><font face="monospace">r</font></code>. Một số nguyên dương được gọi là <strong data-end="276" data-start="263">đẹp</strong> nếu tích các chữ số của nó chia hết cho tổng các chữ số.</p>

<p>Hãy trả về số lượng số <strong>đẹp</strong> trong đoạn từ <code>l</code> đến <code>r</code>, bao gồm cả hai đầu mút.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 10, r = 20</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số đẹp trong đoạn là 10 và 20.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 1, r = 15</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số đẹp trong đoạn là 1, 2, 3, 4, 5, 6, 7, 8, 9 và 10.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= l &lt;= r &lt; 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một số đẹp có tích các chữ số chia hết cho tổng các chữ số. Vì đoạn giá trị lớn, ta đếm bằng digit DP.
>
> Các thừa số nguyên tố của tích chỉ có thể là $2,3,5,7$; tổng các chữ số nhiều nhất là $9$ lần độ dài. Một state lưu vị trí, cờ tight, cờ leading-zero, tổng hiện tại và tích (hoặc số mũ của các thừa số nguyên tố).
>
> Trừ số lượng trên $[1,l-1]$ khỏi $[1,r]$. Các số 0 ở đầu giữ tích bằng $1$ và không cộng vào tổng, nên ta không nhân với $0$ quá sớm.

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
