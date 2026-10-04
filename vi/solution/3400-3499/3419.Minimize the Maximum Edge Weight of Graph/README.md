---
comments: true
difficulty: Medium
rating: 2243
source: Weekly Contest 432 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Binary Search
    - Shortest Path
---

<!-- problem:start -->

# [3419. Minimize the Maximum Edge Weight of Graph](https://leetcode.com/problems/minimize-the-maximum-edge-weight-of-graph)

[中文文档](/solution/3400-3499/3419.Minimize%20the%20Maximum%20Edge%20Weight%20of%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>threshold</code>, cùng một đồ thị <strong>có hướng</strong> có trọng số gồm <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>. Đồ thị được biểu diễn bằng một mảng số nguyên <strong>2D</strong> <code>edges</code>, trong đó <code>edges[i] = [A<sub>i</sub>, B<sub>i</sub>, W<sub>i</sub>]</code> cho biết có một cạnh đi từ node <code>A<sub>i</sub></code> đến node <code>B<sub>i</sub></code> với trọng số <code>W<sub>i</sub></code>.</p>

<p>Bạn phải loại bỏ một số cạnh khỏi đồ thị này (có thể <strong>không</strong> loại bỏ cạnh nào), sao cho đồ thị thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Node 0 phải có thể được đi tới từ mọi node khác.</li>
	<li><strong>Trọng số</strong> cạnh <strong>lớn nhất</strong> trong đồ thị kết quả được <strong>tối thiểu hóa</strong>.</li>
	<li>Mỗi node có <strong>nhiều nhất</strong> <code>threshold</code> cạnh đi ra.</li>
</ul>

<p>Hãy trả về giá trị <strong>nhỏ nhất</strong> có thể có của <strong>trọng số cạnh lớn nhất</strong> sau khi loại bỏ các cạnh cần thiết. Nếu không thể thỏa mãn tất cả điều kiện, hãy trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[1,0,1],[2,0,2],[3,0,1],[4,3,1],[2,1,1]], threshold = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3419.Minimize%20the%20Maximum%20Edge%20Weight%20of%20Graph/images/s-1.png" style="width: 300px; height: 233px;" /></p>

<p>Loại bỏ cạnh <code>2 -&gt; 0</code>. Trọng số lớn nhất trong các cạnh còn lại là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[0,1,1],[0,2,2],[0,3,1],[0,4,1],[1,2,1],[1,4,1]], threshold = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Không thể đi tới node 0 từ node 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[1,2,1],[1,3,3],[1,4,5],[2,3,2],[3,4,2],[4,0,1]], threshold = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3419.Minimize%20the%20Maximum%20Edge%20Weight%20of%20Graph/images/s2-1.png" style="width: 300px; height: 267px;" /></p>

<p>Loại bỏ các cạnh <code>1 -&gt; 3</code> và <code>1 -&gt; 4</code>. Trọng số lớn nhất trong các cạnh còn lại là 2.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[1,2,1],[1,3,3],[1,4,5],[2,3,2],[4,0,1]], threshold = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= threshold &lt;= n - 1</code></li>
	<li><code>1 &lt;= edges.length &lt;= min(10<sup>5</sup>, n * (n - 1) / 2).</code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= A<sub>i</sub>, B<sub>i</sub> &lt; n</code></li>
	<li><code>A<sub>i</sub> != B<sub>i</sub></code></li>
	<li><code>1 &lt;= W<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
	<li>Có thể có nhiều cạnh giữa một cặp node, nhưng chúng phải có trọng số khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi node phải có thể đi tới $0$, mỗi node chỉ được giữ lại nhiều nhất $\textit{threshold}$ cạnh đi ra, đồng thời cần tối thiểu hóa trọng số lớn nhất được giữ lại. Với $n\le 10^5$, không thể liệt kê tất cả các tập con của các cạnh.
>
> Bài toán tối thiểu hóa một giá trị lớn nhất gợi ý dùng binary search. Với một giá trị ứng viên $x$, ta chỉ giữ các cạnh có trọng số $\le x$ và kiểm tra xem có thể chọn một đồ thị con thỏa giới hạn bậc mà vẫn cho phép mọi node đi tới $0$ hay không.
>
> Đảo chiều các cạnh rồi BFS/DFS từ $0$, khi đó mỗi node có nhiều nhất $\textit{threshold}$ cạnh đi vào trong đồ thị đảo. Nếu mọi node đều được duyệt tới thì $x$ khả thi; ta dùng binary search để tìm $x$ nhỏ nhất như vậy.

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
