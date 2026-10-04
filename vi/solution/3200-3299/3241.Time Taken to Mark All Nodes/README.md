---
comments: true
difficulty: Hard
rating: 2521
source: Biweekly Contest 136 Q4
tags:
    - Tree
    - Depth-First Search
    - Graph
    - Dynamic Programming
    - Tree DP
---

<!-- problem:start -->

# [3241. Time Taken to Mark All Nodes](https://leetcode.com/problems/time-taken-to-mark-all-nodes)

[中文文档](/solution/3200-3299/3241.Time%20Taken%20to%20Mark%20All%20Nodes/README.md)

## Mô tả

<!-- description:start -->
<p>Có một cây <strong>vô hướng</strong> gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cung cấp một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh nối node <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> trong cây.</p>

<p>Ban đầu, <strong>tất cả</strong> node đều <strong>chưa được đánh dấu</strong>. Với mỗi node <code>i</code>:</p>

<ul>
    <li>Nếu <code>i</code> là số lẻ, node sẽ được đánh dấu tại thời điểm <code>x</code> nếu có <strong>ít nhất</strong> một node <em>kề</em> với nó được đánh dấu tại thời điểm <code>x - 1</code>.</li>
    <li>Nếu <code>i</code> là số chẵn, node sẽ được đánh dấu tại thời điểm <code>x</code> nếu có <strong>ít nhất</strong> một node <em>kề</em> với nó được đánh dấu tại thời điểm <code>x - 2</code>.</li>
</ul>

<p>Trả về một mảng <code>times</code>, trong đó <code>times[i]</code> là thời điểm tất cả node trong cây được đánh dấu, nếu bạn đánh dấu node <code>i</code> tại thời điểm <code>t = 0</code>.</p>

<p><strong>Lưu ý</strong> rằng đáp án cho mỗi <code>times[i]</code> là <strong>độc lập</strong>, tức là khi đánh dấu node <code>i</code>, tất cả node khác đều <em>chưa được đánh dấu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2]]</span></p>

<p><strong>Đầu ra:</strong> [2,4,3]</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3241.Time%20Taken%20to%20Mark%20All%20Nodes/images/screenshot-2024-06-02-122236.png" style="width: 500px; height: 241px;" /></p>

<ul>
    <li>Với <code>i = 0</code>:

    <ul>
        <li>Node 1 được đánh dấu tại <code>t = 1</code>, còn Node 2 tại <code>t = 2</code>.</li>
    </ul>
    </li>
    <li>Với <code>i = 1</code>:
    <ul>
        <li>Node 0 được đánh dấu tại <code>t = 2</code>, còn Node 2 tại <code>t = 4</code>.</li>
    </ul>
    </li>
    <li>Với <code>i = 2</code>:
    <ul>
        <li>Node 0 được đánh dấu tại <code>t = 2</code>, còn Node 1 tại <code>t = 3</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1]]</span></p>

<p><strong>Đầu ra:</strong> [1,2]</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3241.Time%20Taken%20to%20Mark%20All%20Nodes/images/screenshot-2024-06-02-122249.png" style="width: 500px; height: 257px;" /></p>

<ul>
    <li>Với <code>i = 0</code>:

    <ul>
        <li>Node 1 được đánh dấu tại <code>t = 1</code>.</li>
    </ul>
    </li>
    <li>Với <code>i = 1</code>:
    <ul>
        <li>Node 0 được đánh dấu tại <code>t = 2</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = </span>[[2,4],[0,1],[2,3],[0,2]]</p>

<p><strong>Đầu ra:</strong> [4,6,3,5,5]</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3241.Time%20Taken%20to%20Mark%20All%20Nodes/images/screenshot-2024-06-03-210550.png" style="height: 266px; width: 500px;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>edges.length == n - 1</code></li>
    <li><code>edges[i].length == 2</code></li>
    <li><code>0 &lt;= edges[i][0], edges[i][1] &lt;= n - 1</code></li>
    <li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xuất phát từ mỗi node để đánh dấu cây, ta tốn $1$ hoặc $2$ khi đi qua node lẻ hoặc chẵn, và cần tính thời gian cho từng node bắt đầu. Với $n\le 10^5$, không thể thực hiện một lần DFS cho mỗi node.
>
> Thời gian lớn nhất trong một cây con có thể được tính qua một lượt duyệt từ dưới lên; sau đó, kỹ thuật rerooting kết hợp phần cây bên phía cha để tạo thành chuỗi dài nhất đi ra khỏi cây con hiện tại. Sau hai lượt duyệt, ta biết được mọi đáp án. Hiện chưa có phần cài đặt cây; phần lập luận dưới đây trình bày phác thảo của kỹ thuật rerooting này.

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
