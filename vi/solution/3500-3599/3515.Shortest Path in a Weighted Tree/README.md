---
comments: true
difficulty: Hard
rating: 2312
source: Biweekly Contest 154 Q4
tags:
    - Tree
    - Depth-First Search
    - Binary Indexed Tree
    - Segment Tree
    - Array
---

<!-- problem:start -->

# [3515. Shortest Path in a Weighted Tree](https://leetcode.com/problems/shortest-path-in-a-weighted-tree)

[中文文档](/solution/3500-3599/3515.Shortest%20Path%20in%20a%20Weighted%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một số nguyên <code>n</code> và một cây vô hướng có trọng số, được gốc tại nút 1, gồm <code>n</code> nút được đánh số từ 1 đến <code>n</code>. Cây được biểu diễn bằng một mảng 2D <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> biểu thị một cạnh vô hướng nối nút <code>u<sub>i</sub></code> với nút <code>v<sub>i</sub></code>, có trọng số <code>w<sub>i</sub></code>.</p>

<p>Bạn cũng được cung cấp một mảng số nguyên 2D <code>queries</code> có độ dài <code>q</code>, trong đó mỗi <code>queries[i]</code> thuộc một trong các dạng sau:</p>

<ul>
	<li><code>[1, u, v, w&#39;]</code> &ndash; <strong>Cập nhật</strong> trọng số của cạnh nối các nút <code>u</code> và <code>v</code> thành <code>w&#39;</code>, trong đó <code>(u, v)</code> được đảm bảo là một cạnh có trong <code>edges</code>.</li>
	<li><code>[2, x]</code> &ndash; <strong>Tính</strong> khoảng cách đường đi <strong>ngắn nhất</strong> từ nút gốc 1 đến nút <code>x</code>.</li>
</ul>

<p>Trả về một mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là khoảng cách đường đi <strong>ngắn nhất</strong> từ nút 1 đến <code>x</code> đối với truy vấn <code>i<sup>th</sup></code> có dạng <code>[2, x]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[1,2,7]], queries = [[2,2],[1,1,2,4],[2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[7,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3515.Shortest%20Path%20in%20a%20Weighted%20Tree/images/screenshot-2025-03-13-at-133524.png" style="width: 200px; height: 75px;" /></p>

<ul>
	<li>Truy vấn <code>[2,2]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 2 có độ dài là 7.</li>
	<li>Truy vấn <code>[1,1,2,4]</code>: Trọng số của cạnh <code>(1,2)</code> thay đổi từ 7 thành 4.</li>
	<li>Truy vấn <code>[2,2]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 2 có độ dài là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[1,2,2],[1,3,4]], queries = [[2,1],[2,3],[1,1,3,7],[2,2],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,4,2,7]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3515.Shortest%20Path%20in%20a%20Weighted%20Tree/images/screenshot-2025-03-13-at-132247.png" style="width: 180px; height: 141px;" /></p>

<ul>
	<li>Truy vấn <code>[2,1]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 1 có độ dài là 0.</li>
	<li>Truy vấn <code>[2,3]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 3 có độ dài là 4.</li>
	<li>Truy vấn <code>[1,1,3,7]</code>: Trọng số của cạnh <code>(1,3)</code> thay đổi từ 4 thành 7.</li>
	<li>Truy vấn <code>[2,2]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 2 có độ dài là 2.</li>
	<li>Truy vấn <code>[2,3]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 3 có độ dài là 7.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[1,2,2],[2,3,1],[3,4,5]], queries = [[2,4],[2,3],[1,2,3,3],[2,2],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> [8,3,2,5]</p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3515.Shortest%20Path%20in%20a%20Weighted%20Tree/images/screenshot-2025-03-13-at-133306.png" style="width: 400px; height: 85px;" /></p>

<ul>
	<li>Truy vấn <code>[2,4]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 4 gồm các cạnh <code>(1,2)</code>, <code>(2,3)</code> và <code>(3,4)</code>, với tổng trọng số là <code>2 + 1 + 5 = 8</code>.</li>
	<li>Truy vấn <code>[2,3]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 3 gồm các cạnh <code>(1,2)</code> và <code>(2,3)</code>, với tổng trọng số là <code>2 + 1 = 3</code>.</li>
	<li>Truy vấn <code>[1,2,3,3]</code>: Trọng số của cạnh <code>(2,3)</code> thay đổi từ 1 thành 3.</li>
	<li>Truy vấn <code>[2,2]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 2 có độ dài là 2.</li>
	<li>Truy vấn <code>[2,3]</code>: Đường đi ngắn nhất từ nút gốc 1 đến nút 3 gồm các cạnh <code>(1,2)</code> và <code>(2,3)</code>, với trọng số mới là <code>2 + 3 = 5</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>1 &lt;= queries.length == q &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code> hoặc <code>4</code>
	<ul>
		<li><code>queries[i] == [1, u, v, w&#39;]</code> hoặc,</li>
		<li><code>queries[i] == [2, x]</code></li>
		<li><code>1 &lt;= u, v, x &lt;= n</code></li>
		<li><code data-end="37" data-start="29">(u, v)</code> luôn là một cạnh trong <code data-end="74" data-start="67">edges</code>.</li>
		<li><code>1 &lt;= w&#39; &lt;= 10<sup>4</sup></code></li>
	</ul>
	</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách từ gốc đến các nút thay đổi khi cập nhật cạnh, và $n,q \le 10^5$ khiến việc tính lại từ gốc sau mỗi lần cập nhật là không thể. Đường đi là duy nhất, nên $\textit{dist}(x)$ là tổng trọng số các cạnh trên đường đi từ gốc đến $x$.
>
> Trên Euler tour, việc cập nhật một cạnh sẽ cộng một giá trị vào một đoạn liên tiếp, còn truy vấn là đọc tại một điểm. Fenwick tree hoặc segment tree có thể duy trì các giá trị này.

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
