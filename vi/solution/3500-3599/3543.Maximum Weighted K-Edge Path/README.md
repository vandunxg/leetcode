---
comments: true
difficulty: Medium
rating: 2110
source: Biweekly Contest 156 Q3
tags:
    - Graph
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [3543. Maximum Weighted K-Edge Path](https://leetcode.com/problems/maximum-weighted-k-edge-path)

[中文文档](/solution/3500-3599/3543.Maximum%20Weighted%20K-Edge%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một <strong>đồ thị có hướng không chu trình (DAG)</strong> gồm <code>n</code> node được đánh nhãn từ 0 đến <code>n - 1</code>. Đồ thị được biểu diễn bằng một mảng 2 chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> biểu thị một cạnh có hướng từ node <code>u<sub>i</sub></code> đến <code>v<sub>i</sub></code> với trọng số <code>w<sub>i</sub></code>.</p>

<p>Cho thêm hai số nguyên <code>k</code> và <code>t</code>.</p>

<p>Nhiệm vụ của bạn là xác định tổng trọng số cạnh <strong>lớn nhất</strong> có thể có của một path trong đồ thị sao cho:</p>

<ul>
    <li>Path chứa <strong>chính xác</strong> <code>k</code> cạnh.</li>
    <li>Tổng trọng số các cạnh trong path <strong>nhỏ hơn</strong> <code>t</code> một cách nghiêm ngặt.</li>
</ul>

<p>Trả về tổng trọng số <strong>lớn nhất</strong> có thể có của một path như vậy. Nếu không tồn tại path nào, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,1],[1,2,2]], k = 2, t = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3543.Maximum%20Weighted%20K-Edge%20Path/images/screenshot-2025-04-10-at-061326.png" style="width: 180px; height: 162px;" /></p>

<ul>
    <li>Path duy nhất có <code>k = 2</code> cạnh là <code>0 -&gt; 1 -&gt; 2</code> với trọng số <code>1 + 2 = 3 &lt; t</code>.</li>
    <li>Do đó, tổng trọng số lớn nhất nhỏ hơn <code>t</code> có thể có là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,2],[0,2,3]], k = 1, t = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3543.Maximum%20Weighted%20K-Edge%20Path/images/screenshot-2025-04-10-at-061406.png" style="width: 180px; height: 164px;" /></p>

<ul>
    <li>Có hai path với <code>k = 1</code> cạnh:

    <ul>
        <li><code>0 -&gt; 1</code> với trọng số <code>2 &lt; t</code>.</li>
        <li><code>0 -&gt; 2</code> với trọng số <code>3 = t</code>, không thỏa điều kiện nhỏ hơn <code>t</code>.</li>
    </ul>
    </li>
    <li>Do đó, tổng trọng số lớn nhất nhỏ hơn <code>t</code> có thể có là 2.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,6],[1,2,8]], k = 1, t = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3543.Maximum%20Weighted%20K-Edge%20Path/images/screenshot-2025-04-10-at-061442.png" style="width: 180px; height: 154px;" /></p>

<ul>
    <li>Có hai path với k = 1 cạnh:
    <ul>
        <li><code>0 -&gt; 1</code> với trọng số <code>6 = t</code>, không thỏa điều kiện nhỏ hơn <code>t</code>.</li>
        <li><code>1 -&gt; 2</code> với trọng số <code>8 &gt; t</code>, không thỏa điều kiện nhỏ hơn <code>t</code>.</li>
    </ul>
    </li>
    <li>Vì không có path nào có tổng trọng số thỏa điều kiện nhỏ hơn <code>t</code>, đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 300</code></li>
    <li><code>0 &lt;= edges.length &lt;= 300</code></li>
    <li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
    <li><code>1 &lt;= w<sub>i</sub> &lt;= 10</code></li>
    <li><code>0 &lt;= k &lt;= 300</code></li>
    <li><code>1 &lt;= t &lt;= 600</code></li>
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
> Ta cần một path gồm $k$ cạnh trong DAG sao cho tổng trọng số được tối đa hóa nhưng nhỏ hơn $t$ một cách nghiêm ngặt. Với $n,k \le 300$ và $t \le 600$, trạng thái gồm (đỉnh, số cạnh đã dùng, tổng trọng số) đủ nhỏ.
>
> Dùng DFS hoặc duyệt từ mọi đỉnh bắt đầu. Trong các trạng thái có $e=k$ và $s<t$, chọn $s$ lớn nhất, hoặc trả về $-1$ nếu không có trạng thái nào.

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
