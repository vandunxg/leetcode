---
comments: true
difficulty: Hard
rating: 2461
source: Weekly Contest 448 Q3
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3538. Merge Operations for Minimum Travel Time](https://leetcode.com/problems/merge-operations-for-minimum-travel-time)

[中文文档](/solution/3500-3599/3538.Merge%20Operations%20for%20Minimum%20Travel%20Time/README.md)

## Mô tả

<!-- description:start -->

<p data-end="452" data-start="24">Bạn được cho một con đường thẳng dài <code>l</code> km, một số nguyên <code>n</code>, một số nguyên <code>k</code><strong data-end="83" data-start="78">, </strong>và <strong>hai</strong> mảng số nguyên <code>position</code> và <code>time</code>, mỗi mảng có độ dài <code>n</code>.</p>

<p data-end="452" data-start="24">Mảng <code>position</code> liệt kê vị trí (tính bằng km) của các biển báo theo thứ tự <strong>tăng nghiêm ngặt</strong> (<code>position[0] = 0</code> và <code>position[n - 1] = l</code>).</p>

<p data-end="452" data-start="24">Mỗi <code>time[i]</code> biểu thị thời gian (tính bằng phút) cần để đi 1 km giữa <code>position[i]</code> và <code>position[i + 1]</code>.</p>

<p data-end="593" data-start="454">Bạn <strong>phải</strong> thực hiện <strong>đúng</strong> <code>k</code> thao tác gộp. Trong một lần gộp, bạn có thể chọn <strong>hai</strong> biển báo liền kề tại các chỉ số <code>i</code> và <code>i + 1</code> (với <code>i &gt; 0</code> và <code>i + 1 &lt; n</code>) rồi:</p>

<ul data-end="701" data-start="595">
    <li data-end="624" data-start="595">Cập nhật biển báo tại chỉ số <code>i + 1</code> để thời gian của nó trở thành <code>time[i] + time[i + 1]</code>.</li>
    <li data-end="624" data-start="595">Xóa biển báo tại chỉ số <code>i</code>.</li>
</ul>

<p data-end="846" data-start="703">Trả về <strong>tổng</strong> <strong>thời gian di chuyển</strong> (tính bằng phút) <strong>nhỏ nhất</strong> để đi từ 0 đến <code>l</code> sau <strong>đúng</strong> <code>k</code> lần gộp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 10, n = 4, k = 1, position = [0,3,8,10], time = [5,8,3,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">62</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li data-end="121" data-start="11">
    <p data-end="121" data-start="13">Gộp các biển báo tại chỉ số 1 và 2. Xóa biển báo tại chỉ số 1, rồi đổi thời gian tại chỉ số 2 thành <code>8 + 3 = 11</code>.</p>
    </li>
    <li data-end="144" data-start="15">Sau khi gộp:
    <ul>
        <li data-end="214" data-start="145">Mảng <code>position</code>: <code>[0, 8, 10]</code></li>
        <li data-end="214" data-start="145">Mảng <code>time</code>: <code>[5, 11, 6]</code></li>
        <li data-end="214" data-start="145" style="opacity: 0"> </li>
    </ul>
    </li>
    <li data-end="214" data-start="145">
    <table data-end="386" data-start="231" style="border: 1px solid black;">
        <thead data-end="269" data-start="231">
            <tr data-end="269" data-start="231">
                <th data-end="241" data-start="231" style="border: 1px solid black;">Đoạn</th>
                <th data-end="252" data-start="241" style="border: 1px solid black;">Khoảng cách (km)</th>
                <th data-end="260" data-start="252" style="border: 1px solid black;">Thời gian mỗi km (phút)</th>
                <th data-end="269" data-start="260" style="border: 1px solid black;">Thời gian di chuyển trên đoạn (phút)</th>
            </tr>
        </thead>
        <tbody data-end="386" data-start="309">
            <tr data-end="347" data-start="309">
                <td style="border: 1px solid black;">0 &rarr; 8</td>
                <td style="border: 1px solid black;">8</td>
                <td style="border: 1px solid black;">5</td>
                <td style="border: 1px solid black;">8 &times; 5 = 40</td>
            </tr>
            <tr data-end="386" data-start="348">
                <td style="border: 1px solid black;">8 &rarr; 10</td>
                <td style="border: 1px solid black;">2</td>
                <td style="border: 1px solid black;">11</td>
                <td style="border: 1px solid black;">2 &times; 11 = 22</td>
            </tr>
        </tbody>
    </table>
    </li>
    <li data-end="214" data-start="145">Tổng thời gian di chuyển: <code>40 + 22 = 62</code>, đây là thời gian nhỏ nhất có thể đạt được sau đúng 1 lần gộp.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 5, n = 5, k = 1, position = [0,1,2,3,5], time = [8,3,9,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">34</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li data-end="567" data-start="438">Gộp các biển báo tại chỉ số 1 và 2. Xóa biển báo tại chỉ số 1, rồi đổi thời gian tại chỉ số 2 thành <code>3 + 9 = 12</code>.</li>
    <li data-end="755" data-start="568">Sau khi gộp:
    <ul>
        <li data-end="755" data-start="568">Mảng <code>position</code>: <code>[0, 2, 3, 5]</code></li>
        <li data-end="755" data-start="568">Mảng <code>time</code>: <code>[8, 12, 3, 3]</code></li>
        <li data-end="755" data-start="568" style="opacity: 0"> </li>
    </ul>
    </li>
    <li data-end="755" data-start="568">
    <table data-end="966" data-start="772" style="border: 1px solid black;">
        <thead data-end="810" data-start="772">
            <tr data-end="810" data-start="772">
                <th data-end="782" data-start="772" style="border: 1px solid black;">Đoạn</th>
                <th data-end="793" data-start="782" style="border: 1px solid black;">Khoảng cách (km)</th>
                <th data-end="801" data-start="793" style="border: 1px solid black;">Thời gian mỗi km (phút)</th>
                <th data-end="810" data-start="801" style="border: 1px solid black;">Thời gian di chuyển trên đoạn (phút)</th>
            </tr>
        </thead>
        <tbody data-end="966" data-start="850">
            <tr data-end="888" data-start="850">
                <td style="border: 1px solid black;">0 &rarr; 2</td>
                <td style="border: 1px solid black;">2</td>
                <td style="border: 1px solid black;">8</td>
                <td style="border: 1px solid black;">2 &times; 8 = 16</td>
            </tr>
            <tr data-end="927" data-start="889">
                <td style="border: 1px solid black;">2 &rarr; 3</td>
                <td style="border: 1px solid black;">1</td>
                <td style="border: 1px solid black;">12</td>
                <td style="border: 1px solid black;">1 &times; 12 = 12</td>
            </tr>
            <tr data-end="966" data-start="928">
                <td style="border: 1px solid black;">3 &rarr; 5</td>
                <td style="border: 1px solid black;">2</td>
                <td style="border: 1px solid black;">3</td>
                <td style="border: 1px solid black;">2 &times; 3 = 6</td>
            </tr>
        </tbody>
    </table>
    </li>
    <li data-end="755" data-start="568">Tổng thời gian di chuyển: <code>16 + 12 + 6 = 34</code><b>, </b>đây là thời gian nhỏ nhất có thể đạt được sau đúng 1 lần gộp.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="35" data-start="15"><code>1 &lt;= l &lt;= 10<sup>5</sup></code></li>
    <li data-end="52" data-start="36"><code>2 &lt;= n &lt;= min(l + 1, 50)</code></li>
    <li data-end="81" data-start="53"><code>0 &lt;= k &lt;= min(n - 2, 10)</code></li>
    <li data-end="81" data-start="53"><code>position.length == n</code></li>
    <li data-end="81" data-start="53"><code>position[0] = 0</code> và <code>position[n - 1] = l</code></li>
    <li data-end="200" data-start="80"><code>position</code> được sắp xếp theo thứ tự tăng nghiêm ngặt.</li>
    <li data-end="81" data-start="53"><code>time.length == n</code></li>
    <li data-end="81" data-start="53"><code>1 &lt;= time[i] &lt;= 100​</code></li>
    <li data-end="81" data-start="53"><code>1 &lt;= sum(time) &lt;= 100</code>​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n \le 50$ và phải thực hiện đúng $k \le 10$ lần gộp, không thể liệt kê thứ tự gộp. Sau một lần gộp, tốc độ của một đoạn là tổng các giá trị $\textit{time}$ được gộp, còn khoảng cách là khoảng cách giữa các $\textit{position}$ còn lại.
>
> Áp dụng DP theo biển báo cuối cùng được giữ lại, số lần gộp đã sử dụng và tốc độ kế thừa từ một tổng $\textit{time}$ liên tiếp. Ta liệt kê số biển báo mà đoạn hiện tại gộp vào. Ba chiều này là đủ.

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
