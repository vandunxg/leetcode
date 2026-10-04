---
comments: true
difficulty: Hard
rating: 2330
source: Biweekly Contest 161 Q4
tags:
    - Bit Manipulation
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [3621. Number of Integers With Popcount-Depth Equal to K I](https://leetcode.com/problems/number-of-integers-with-popcount-depth-equal-to-k-i)

[中文文档](/solution/3600-3699/3621.Number%20of%20Integers%20With%20Popcount-Depth%20Equal%20to%20K%20I/README.md)

## Mô tả

<!-- description:start -->
<p>Bạn được cho hai số nguyên <code>n</code> và <code>k</code>.</p>

<p>Với mọi số nguyên dương <code>x</code>, định nghĩa dãy sau:</p>

<ul>
    <li><code>p<sub>0</sub> = x</code></li>
    <li><code>p<sub>i+1</sub> = popcount(p<sub>i</sub>)</code> với mọi <code>i &gt;= 0</code>, trong đó <code>popcount(y)</code> là số lượng bit được bật (bit 1) trong biểu diễn nhị phân của <code>y</code>.</li>
</ul>

<p>Dãy này cuối cùng sẽ đạt đến giá trị 1.</p>

<p><strong>Độ sâu popcount</strong> của <code>x</code> được định nghĩa là số nguyên <strong>nhỏ nhất</strong> <code>d &gt;= 0</code> sao cho <code>p<sub>d</sub> = 1</code>.</p>

<p>Ví dụ, nếu <code>x = 7</code> (biểu diễn nhị phân <code>&quot;111&quot;</code>). Khi đó, dãy là: <code>7 &rarr; 3 &rarr; 2 &rarr; 1</code>, nên độ sâu popcount của 7 là 3.</p>

<p>Nhiệm vụ của bạn là xác định số lượng số nguyên trong đoạn <code>[1, n]</code> có độ sâu popcount <strong>chính xác</strong> bằng <code>k</code>.</p>

<p>Trả về số lượng số nguyên như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên sau trong đoạn <code>[1, 4]</code> có độ sâu popcount chính xác bằng 1:</p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th align="center" style="border: 1px solid black;">x</th>
            <th align="center" style="border: 1px solid black;">Nhị phân</th>
            <th align="left" style="border: 1px solid black;">Dãy</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td align="center" style="border: 1px solid black;">2</td>
            <td align="center" style="border: 1px solid black;"><code>&quot;10&quot;</code></td>
            <td align="left" style="border: 1px solid black;"><code>2 &rarr; 1</code></td>
        </tr>
        <tr>
            <td align="center" style="border: 1px solid black;">4</td>
            <td align="center" style="border: 1px solid black;"><code>&quot;100&quot;</code></td>
            <td align="left" style="border: 1px solid black;"><code>4 &rarr; 1</code></td>
        </tr>
    </tbody>
</table>

<p>Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên sau trong đoạn <code>[1, 7]</code> có độ sâu popcount chính xác bằng 2:</p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;">x</th>
            <th style="border: 1px solid black;">Nhị phân</th>
            <th style="border: 1px solid black;">Dãy</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;"><code>&quot;11&quot;</code></td>
            <td style="border: 1px solid black;"><code>3 &rarr; 2 &rarr; 1</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">5</td>
            <td style="border: 1px solid black;"><code>&quot;101&quot;</code></td>
            <td style="border: 1px solid black;"><code>5 &rarr; 2 &rarr; 1</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">6</td>
            <td style="border: 1px solid black;"><code>&quot;110&quot;</code></td>
            <td style="border: 1px solid black;"><code>6 &rarr; 2 &rarr; 1</code></td>
        </tr>
    </tbody>
</table>

<p>Vì vậy, đáp án là 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 10<sup>15</sup></code></li>
    <li><code>0 &lt;= k &lt;= 5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Độ sâu popcount là số lần thay $x$ bằng $\mathrm{popcount}(x)$ cho đến khi nó trở thành $1$. $n$ quá lớn để duyệt toàn bộ đoạn $[1,n]$.
>
> Độ sâu rất nhỏ vì một lần popcount rút gọn $x$ thành độ dài bit của nó. Digit DP đếm các số nguyên $\le n$ có số lượng bit 1 bằng $c$; sau đó đối chiếu các $c$ này với $k$.
>
> Tính trước $\textit{depth}(c)$ cho mọi giá trị popcount khả thi. Xử lý $k=0$ như trường hợp duy nhất là $1$. Digit DP nhị phân trên $n$ cộng tất cả các $c$ thỏa mãn $\textit{depth}(c)=k-1$, vì thêm một lần popcount sẽ làm độ sâu tăng thêm một.

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
