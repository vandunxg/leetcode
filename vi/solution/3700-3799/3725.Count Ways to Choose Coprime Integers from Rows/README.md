---
comments: true
difficulty: Hard
rating: 1981
source: Biweekly Contest 168 Q4
tags:
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
    - Matrix
    - Number Theory
---

<!-- problem:start -->

# [3725. Count Ways to Choose Coprime Integers from Rows](https://leetcode.com/problems/count-ways-to-choose-coprime-integers-from-rows)

[中文文档](/solution/3700-3799/3725.Count%20Ways%20to%20Choose%20Coprime%20Integers%20from%20Rows/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <code>m x n</code> gồm các số nguyên dương <code>mat</code>.</p>

<p>Trả về một số nguyên biểu thị số cách chọn <strong>chính xác một</strong> số nguyên từ mỗi hàng của <code>mat</code> sao cho <strong>ước chung lớn nhất</strong> của tất cả các số nguyên được chọn bằng 1.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mat = [[1,2],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <tbody>
        <tr>
            <th align="center" style="border: 1px solid black;">Số nguyên được chọn ở hàng đầu tiên</th>
            <th align="center" style="border: 1px solid black;">Số nguyên được chọn ở hàng thứ hai</th>
            <th align="center" style="border: 1px solid black;">Ước chung lớn nhất của các số nguyên được chọn</th>
        </tr>
        <tr>
            <td align="center" style="border: 1px solid black;">1</td>
            <td align="center" style="border: 1px solid black;">3</td>
            <td align="center" style="border: 1px solid black;">1</td>
        </tr>
        <tr>
            <td align="center" style="border: 1px solid black;">1</td>
            <td align="center" style="border: 1px solid black;">4</td>
            <td align="center" style="border: 1px solid black;">1</td>
        </tr>
        <tr>
            <td align="center" style="border: 1px solid black;">2</td>
            <td align="center" style="border: 1px solid black;">3</td>
            <td align="center" style="border: 1px solid black;">1</td>
        </tr>
        <tr>
            <td align="center" style="border: 1px solid black;">2</td>
            <td align="center" style="border: 1px solid black;">4</td>
            <td align="center" style="border: 1px solid black;">2</td>
        </tr>
    </tbody>
</table>

<p>Có 3 tổ hợp có ước chung lớn nhất bằng 1. Do đó, đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mat = [[2,2],[2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi tổ hợp đều có ước chung lớn nhất bằng 2. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == mat.length &lt;= 150</code></li>
    <li><code>1 &lt;= n == mat[i].length &lt;= 150</code></li>
    <li><code>1 &lt;= mat[i][j] &lt;= 150</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta chọn chính xác một số nguyên từ mỗi hàng và cần đếm số cách theo modulo $10^9+7$; việc liệt kê các bộ sẽ tăng theo cấp số mũ với số hàng. Vì chỉ có $\gcd$ tổng thể là quan trọng, ta dùng quy hoạch động theo từng hàng với trạng thái là $\gcd$ hiện tại, rồi cộng số cách có $\gcd$ cuối cùng bằng $1$.

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
