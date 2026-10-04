---
comments: true
difficulty: Hard
rating: 2352
source: Biweekly Contest 155 Q4
tags:
    - Bit Manipulation
    - Graph
    - Topological Sort
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [3530. Maximum Profit from Valid Topological Order in DAG](https://leetcode.com/problems/maximum-profit-from-valid-topological-order-in-dag)

[中文文档](/solution/3500-3599/3530.Maximum%20Profit%20from%20Valid%20Topological%20Order%20in%20DAG/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <strong>đồ thị có hướng không chu trình (DAG)</strong> gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code>, được biểu diễn bằng một mảng 2D <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh có hướng từ nút <code>u<sub>i</sub></code> đến nút <code>v<sub>i</sub></code>. Mỗi nút có một <strong>điểm số</strong> tương ứng trong mảng <code>score</code>, trong đó <code>score[i]</code> là điểm số của nút <code>i</code>.</p>

<p>Bạn phải xử lý các nút theo một <strong>thứ tự topo hợp lệ</strong>. Mỗi nút được gán một <strong>vị trí bắt đầu từ 1</strong> trong thứ tự xử lý.</p>

<p><strong>Lợi nhuận</strong> được tính bằng tổng tích của điểm số mỗi nút với vị trí của nút đó trong thứ tự.</p>

<p>Trả về <strong>lợi nhuận lớn nhất </strong>có thể đạt được với một thứ tự topo tối ưu.</p>

<p><strong>Thứ tự topo</strong> của một DAG là một thứ tự tuyến tính của các nút sao cho với mọi cạnh có hướng <code>u &rarr; v</code>, nút <code>u</code> xuất hiện trước nút <code>v</code> trong thứ tự.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1]], score = [2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3530.Maximum%20Profit%20from%20Valid%20Topological%20Order%20in%20DAG/images/screenshot-2025-03-11-at-021131.png" style="width: 200px; height: 89px;" /></p>

<p>Nút 1 phụ thuộc vào nút 0, nên một thứ tự hợp lệ là <code>[0, 1]</code>.</p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;">Nút</th>
            <th style="border: 1px solid black;">Thứ tự xử lý</th>
            <th style="border: 1px solid black;">Điểm số</th>
            <th style="border: 1px solid black;">Hệ số</th>
            <th style="border: 1px solid black;">Tính lợi nhuận</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">1st</td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">2 &times; 1 = 2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">2nd</td>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">3 &times; 2 = 6</td>
        </tr>
    </tbody>
</table>

<p>Tổng lợi nhuận lớn nhất có thể đạt được trên mọi thứ tự topo hợp lệ là <code>2 + 6 = 8</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[0,2]], score = [1,6,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3530.Maximum%20Profit%20from%20Valid%20Topological%20Order%20in%20DAG/images/screenshot-2025-03-11-at-023558.png" style="width: 200px; height: 124px;" /></p>

<p>Các nút 1 và 2 phụ thuộc vào nút 0, nên thứ tự hợp lệ tối ưu nhất là <code>[0, 2, 1]</code>.</p>

<table data-end="1197" data-start="851" node="[object Object]" style="border: 1px solid black;">
    <thead data-end="920" data-start="851">
        <tr data-end="920" data-start="851">
            <th data-end="858" data-start="851" style="border: 1px solid black;">Nút</th>
            <th data-end="877" data-start="858" style="border: 1px solid black;">Thứ tự xử lý</th>
            <th data-end="885" data-start="877" style="border: 1px solid black;">Điểm số</th>
            <th data-end="898" data-start="885" style="border: 1px solid black;">Hệ số</th>
            <th data-end="920" data-start="898" style="border: 1px solid black;">Tính lợi nhuận</th>
        </tr>
    </thead>
    <tbody data-end="1197" data-start="991">
        <tr data-end="1059" data-start="991">
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;">1st</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">1 &times; 1 = 1</td>
        </tr>
        <tr data-end="1128" data-start="1060">
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">2nd</td>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">3 &times; 2 = 6</td>
        </tr>
        <tr data-end="1197" data-start="1129">
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">3rd</td>
            <td style="border: 1px solid black;">6</td>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">6 &times; 3 = 18</td>
        </tr>
    </tbody>
</table>

<p>Tổng lợi nhuận lớn nhất có thể đạt được trên mọi thứ tự topo hợp lệ là <code>1 + 6 + 18 = 25</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == score.length &lt;= 22</code></li>
    <li><code>1 &lt;= score[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= edges.length &lt;= n * (n - 1) / 2</code></li>
    <li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh có hướng từ nút <code>u<sub>i</sub></code> đến nút <code>v<sub>i</sub></code>.</li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
    <li>Đồ thị đầu vào được <strong>đảm bảo</strong> là một <strong>DAG</strong>.</li>
    <li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có số lượng thứ tự topo tăng theo cấp số mũ, nhưng $n \le 22$ cho phép dùng bit mask để biểu diễn tập các nút đã xử lý. Lợi nhuận là tích của $\textit{score}[i]$ với vị trí của nút.
>
> Gọi $f[S]$ là lợi nhuận tốt nhất sau khi xử lý tập $S$. Thử một đỉnh $v \notin S$ mà tất cả các nút kề vào nó đều nằm trong $S$, rồi cộng thêm $\textit{score}[v] \cdot (|S|+1)$. Đáp án là $f[U]$.

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
