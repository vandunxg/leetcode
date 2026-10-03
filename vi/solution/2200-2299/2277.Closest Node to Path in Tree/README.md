---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Array
---

<!-- problem:start -->

# [2277. Closest Node to Path in Tree 🔒](https://leetcode.com/problems/closest-node-to-path-in-tree)

[中文文档](/solution/2200-2299/2277.Closest%20Node%20to%20Path%20in%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code> biểu thị số lượng node trong một cây, được đánh số từ <code>0</code> đến <code>n - 1</code> (<strong>bao gồm cả hai đầu</strong>). Bạn cũng được cho một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [node1<sub>i</sub>, node2<sub>i</sub>]</code> biểu thị có một cạnh <strong>hai chiều</strong> nối <code>node1<sub>i</sub></code> và <code>node2<sub>i</sub></code> trong cây.</p>

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>query</code> có độ dài <code>m</code>, trong đó <code>query[i] = [start<sub>i</sub>, end<sub>i</sub>, node<sub>i</sub>]</code> nghĩa là với <code>i<sup>th</sup></code> truy vấn, bạn cần tìm node trên đường đi từ <code>start<sub>i</sub></code> đến <code>end<sub>i</sub></code> <strong>gần nhất</strong> với <code>node<sub>i</sub></code>.</p>

<p>Trả về <em>một mảng số nguyên </em><code>answer</code><em> có độ dài </em><code>m</code><em>, trong đó </em><code>answer[i]</code><em> là đáp án của </em><code>i<sup>th</sup></code><em> truy vấn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2277.Closest%20Node%20to%20Path%20in%20Tree/images/image-20220514132158-1.png" style="width: 300px; height: 211px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1],[0,2],[0,3],[1,4],[2,5],[2,6]], query = [[5,3,4],[5,3,6]]
<strong>Đầu ra:</strong> [0,2]
<strong>Giải thích:</strong>
Đường đi từ node 5 đến node 3 gồm các node 5, 2, 0 và 3.
Khoảng cách giữa node 4 và node 0 là 2.
Node 0 là node trên đường đi gần node 4 nhất, vì vậy đáp án của truy vấn đầu tiên là 0.
Khoảng cách giữa node 6 và node 2 là 1.
Node 2 là node trên đường đi gần node 6 nhất, vì vậy đáp án của truy vấn thứ hai là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2277.Closest%20Node%20to%20Path%20in%20Tree/images/image-20220514132318-2.png" style="width: 300px; height: 89px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1],[1,2]], query = [[0,1,2]]
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong>
Đường đi từ node 0 đến node 1 gồm các node 0, 1.
Khoảng cách giữa node 2 và node 1 là 1.
Node 1 là node trên đường đi gần node 2 nhất, vì vậy đáp án của truy vấn đầu tiên là 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2277.Closest%20Node%20to%20Path%20in%20Tree/images/image-20220514132333-3.png" style="width: 300px; height: 89px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1],[1,2]], query = [[0,0,0]]
<strong>Đầu ra:</strong> [0]
<strong>Giải thích:</strong>
Đường đi từ node 0 đến node 0 chỉ gồm node 0.
Vì 0 là node duy nhất trên đường đi, đáp án của truy vấn đầu tiên là 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= node1<sub>i</sub>, node2<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>node1<sub>i</sub> != node2<sub>i</sub></code></li>
	<li><code>1 &lt;= query.length &lt;= 1000</code></li>
	<li><code>query[i].length == 3</code></li>
	<li><code>0 &lt;= start<sub>i</sub>, end<sub>i</sub>, node<sub>i</sub> &lt;= n - 1</code></li>
	<li>Đồ thị là một cây.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu tìm node gần một $node$ cho trước nhất trên đường đi $start\to end$. Vì $n$ và số lượng truy vấn đều là $10^3$, BFS cho mỗi truy vấn vẫn đủ nhanh, nhưng các đường đi sẽ thường xuyên được dựng lại. Trên cây, node gần nhất là node có độ sâu lớn nhất trong ba node $\mathrm{LCA}(start,end)$, $\mathrm{LCA}(start,node)$ và $\mathrm{LCA}(end,node)$.
>
> Sau khi dùng binary lifting, mỗi truy vấn tính ba LCA này. Các tab không có phần cài đặt; cả binary lifting lẫn BFS cho từng truy vấn đều đáp ứng được giới hạn.

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
