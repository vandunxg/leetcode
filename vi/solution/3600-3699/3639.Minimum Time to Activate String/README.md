---
comments: true
difficulty: Medium
rating: 1853
source: Weekly Contest 461 Q3
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3639. Minimum Time to Activate String](https://leetcode.com/problems/minimum-time-to-activate-string)

[中文文档](/solution/3600-3699/3639.Minimum%20Time%20to%20Activate%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> có độ dài <code>n</code> và một mảng số nguyên <code>order</code>, trong đó <code>order</code> là một <strong><span data-keyword="permutation">hoán vị</span></strong> của các số trong phạm vi <code>[0, n - 1]</code>.</p>

<p>Bắt đầu từ thời điểm <code>t = 0</code>, ở mỗi bước thời gian, hãy thay ký tự tại chỉ số <code>order[t]</code> trong <code>s</code> bằng <code>&#39;*&#39;</code>.</p>

<p>Một <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong> là <strong>hợp lệ</strong> nếu chứa <strong>ít nhất</strong> một <code>&#39;*&#39;</code>.</p>

<p>Một chuỗi là <strong>active</strong> nếu tổng số <strong>chuỗi con hợp lệ</strong> lớn hơn hoặc bằng <code>k</code>.</p>

<p>Trả về thời điểm <strong>nhỏ nhất</strong> <code>t</code> mà chuỗi <code>s</code> trở thành <strong>active</strong>. Nếu không thể, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;, order = [1,0,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>t</code></th>
            <th style="border: 1px solid black;"><code>order[t]</code></th>
            <th style="border: 1px solid black;">Chuỗi <code>s</code> sau khi thay đổi</th>
            <th style="border: 1px solid black;">Các chuỗi con hợp lệ</th>
            <th style="border: 1px solid black;">Số lượng</th>
            <th style="border: 1px solid black;">Active<br />
            (Số lượng &gt;= k)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;"><code>&quot;a*c&quot;</code></td>
            <td style="border: 1px solid black;"><code>&quot;*&quot;</code>, <code>&quot;a*&quot;</code>, <code>&quot;*c&quot;</code>, <code>&quot;a*c&quot;</code></td>
            <td style="border: 1px solid black;">4</td>
            <td style="border: 1px solid black;">Có</td>
        </tr>
    </tbody>
</table>

<p>Chuỗi <code>s</code> trở thành active tại <code>t = 0</code>. Do đó, đáp án là 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cat&quot;, order = [0,2,1], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>t</code></th>
            <th style="border: 1px solid black;"><code>order[t]</code></th>
            <th style="border: 1px solid black;">Chuỗi <code>s</code> sau khi thay đổi</th>
            <th style="border: 1px solid black;">Các chuỗi con hợp lệ</th>
            <th style="border: 1px solid black;">Số lượng</th>
            <th style="border: 1px solid black;">Active<br />
            (Số lượng &gt;= k)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;"><code>&quot;*at&quot;</code></td>
            <td style="border: 1px solid black;"><code>&quot;*&quot;</code>, <code>&quot;*a&quot;</code>, <code>&quot;*at&quot;</code></td>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">Không</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;"><code>&quot;*a*&quot;</code></td>
            <td style="border: 1px solid black;"><code>&quot;*&quot;</code>, <code>&quot;*a&quot;</code>, <code>&quot;<code inline="">*a*&quot;</code></code>, <code>&quot;<code inline="">a*&quot;</code></code>, <code>&quot;*&quot;</code></td>
            <td style="border: 1px solid black;">5</td>
            <td style="border: 1px solid black;">Không</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;"><code>&quot;***&quot;</code></td>
            <td style="border: 1px solid black;">Mọi chuỗi con (chứa <code>&#39;*&#39;</code>)</td>
            <td style="border: 1px solid black;">6</td>
            <td style="border: 1px solid black;">Có</td>
        </tr>
    </tbody>
</table>

<p>Chuỗi <code>s</code> trở thành active tại <code>t = 2</code>. Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;xy&quot;, order = [0,1], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ngay cả sau khi thực hiện tất cả các lần thay thế, vẫn không thể có được <code>k = 4</code> chuỗi con hợp lệ. Do đó, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>order.length == n</code></li>
    <li><code>0 &lt;= order[i] &lt;= n - 1</code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
    <li><code>order</code> là một hoán vị của các số nguyên từ 0 đến <code>n - 1</code>.</li>
    <li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các ký tự lần lượt biến thành dấu sao theo một thứ tự cho trước. Tính khả thi tăng đơn điệu theo thời gian, vì vậy ta có thể dùng tìm kiếm nhị phân để tìm thời điểm đầu tiên điều kiện active với độ dài-$k$ được thỏa mãn.
>
> Với một $t$ ứng viên, coi $t+1$ vị trí đầu tiên là các dấu sao và đếm các chuỗi con được bao phủ dựa trên những khoảng trống giữa các dấu sao liên tiếp.
>
> Nếu số lượng đạt ngưỡng, ta tìm thời điểm nhỏ hơn. Tương đương, ta lần lượt chèn các dấu sao theo thứ tự, duy trì các khoảng trống trong một tập hợp có thứ tự và dừng lại ngay khi đạt điều kiện.

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
