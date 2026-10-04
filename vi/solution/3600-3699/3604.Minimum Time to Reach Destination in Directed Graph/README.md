---
comments: true
difficulty: Medium
rating: 1844
source: Biweekly Contest 160 Q3
tags:
    - Graph
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3604. Minimum Time to Reach Destination in Directed Graph](https://leetcode.com/problems/minimum-time-to-reach-destination-in-directed-graph)

[中文文档](/solution/3600-3699/3604.Minimum%20Time%20to%20Reach%20Destination%20in%20Directed%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một đồ thị <strong>có hướng</strong> gồm <code>n</code> nút được đánh số từ 0 đến <code>n - 1</code>. Đồ thị được biểu diễn bằng một mảng 2D <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, start<sub>i</sub>, end<sub>i</sub>]</code> biểu thị một cạnh đi từ nút <code>u<sub>i</sub></code> đến nút <code>v<sub>i</sub></code>, <strong>chỉ</strong> có thể được sử dụng tại thời điểm nguyên <code>t</code> thỏa mãn <code>start<sub>i</sub> &lt;= t &lt;= end<sub>i</sub></code>.</p>

<p>Bạn bắt đầu tại nút 0 ở thời điểm 0.</p>

<p>Trong một đơn vị thời gian, bạn có thể:</p>

<ul>
    <li>Chờ tại nút hiện tại mà không di chuyển, hoặc</li>
    <li>Đi theo một cạnh đi ra từ nút hiện tại nếu thời điểm hiện tại <code>t</code> thỏa mãn <code>start<sub>i</sub> &lt;= t &lt;= end<sub>i</sub></code>.</li>
</ul>

<p>Trả về <strong>thời gian tối thiểu</strong> cần thiết để đến nút <code>n - 1</code>. Nếu không thể đến được, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,0,1],[1,2,2,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3604.Minimum%20Time%20to%20Reach%20Destination%20in%20Directed%20Graph/images/screenshot-2025-06-06-at-004535.png" style="width: 150px; height: 141px;" /></p>

<p>Đường đi tối ưu là:</p>

<ul>
    <li>Ở thời điểm <code>t = 0</code>, đi theo cạnh <code>(0 &rarr; 1)</code> có thể sử dụng từ 0 đến 1. Bạn đến nút 1 ở thời điểm <code>t = 1</code>, sau đó chờ đến <code>t = 2</code>.</li>
    <li>Ở thời điểm <code>t = <code>2</code></code>, đi theo cạnh <code>(1 &rarr; 2)</code> có thể sử dụng từ 2 đến 5. Bạn đến nút 2 ở thời điểm 3.</li>
</ul>

<p>Do đó, thời gian tối thiểu để đến nút 2 là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1,0,3],[1,3,7,8],[0,2,1,5],[2,3,4,7]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3604.Minimum%20Time%20to%20Reach%20Destination%20in%20Directed%20Graph/images/screenshot-2025-06-06-at-004757.png" style="width: 170px; height: 219px;" /></p>

<p>Đường đi tối ưu là:</p>

<ul>
    <li>Chờ tại nút 0 đến thời điểm <code>t = 1</code>, sau đó đi theo cạnh <code>(0 &rarr; 2)</code> có thể sử dụng từ 1 đến 5. Bạn đến nút 2 ở <code>t = 2</code>.</li>
    <li>Chờ tại nút 2 đến thời điểm <code>t = 4</code>, sau đó đi theo cạnh <code>(2 &rarr; 3)</code> có thể sử dụng từ 4 đến 7. Bạn đến nút 3 ở <code>t = 5</code>.</li>
</ul>

<p>Do đó, thời gian tối thiểu để đến nút 3 là 5.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[1,0,1,3],[1,2,3,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3604.Minimum%20Time%20to%20Reach%20Destination%20in%20Directed%20Graph/images/screenshot-2025-06-06-at-004914.png" style="width: 150px; height: 145px;" /></p>

<ul>
    <li>Vì nút 0 không có cạnh đi ra nên không thể đến nút 2. Do đó, kết quả là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
    <li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, start<sub>i</sub>, end<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
    <li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
    <li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các cạnh chỉ có thể sử dụng trong $[\textit{start},\textit{end}]$. BFS bỏ qua các khoảng thời gian sẽ cho thời điểm đến sai. Ta cần thời điểm sớm nhất đến mỗi đỉnh.
>
> Khi đến $u$ ở thời điểm $t$, ta đi theo cạnh $(u,v,s,e)$ tại thời điểm $\max(t,s)$ nếu thời điểm đó không vượt quá $e$, nên đến $v$ ở thời điểm $\max(t,s)+1$. Nếu bỏ lỡ khoảng thời gian, ta loại cạnh đó.
>
> Đây là bài toán tìm đường đi ngắn nhất với các time window. Heap lấy các đỉnh theo thời điểm đến rồi relax các cạnh đi ra hợp lệ. Lần đầu $n-1$ được lấy khỏi heap là đáp án; nếu heap rỗng thì trả về $-1$.

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
