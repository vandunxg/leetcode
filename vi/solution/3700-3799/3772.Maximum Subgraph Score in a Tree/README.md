---
comments: true
difficulty: Hard
rating: 2234
source: Weekly Contest 479 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3772. Maximum Subgraph Score in a Tree](https://leetcode.com/problems/maximum-subgraph-score-in-a-tree)

[中文文档](/solution/3700-3799/3772.Maximum%20Subgraph%20Score%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <strong>cây vô hướng</strong> có <code>n</code> node, được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối node <code>a<sub>i</sub></code> với node <code>b<sub>i</sub></code> trong cây.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>good</code> có độ dài <code>n</code>, trong đó <code>good[i]</code> bằng 1 nếu node thứ <code>i<sup>th</sup></code> là node tốt, và bằng 0 nếu node đó là node xấu.</p>

<p>Định nghĩa <strong>score</strong> của một <strong>đồ thị con</strong> là số node tốt trừ đi số node xấu trong đồ thị con đó.</p>

<p>Với mỗi node <code>i</code>, hãy tìm <strong>score</strong> lớn nhất trong tất cả <strong>đồ thị con liên thông</strong> chứa node <code>i</code>.</p>

<p>Trả về một mảng gồm <code>n</code> số nguyên, trong đó phần tử thứ <code>i<sup>th</sup></code> là <strong>score</strong> lớn nhất của node <code>i</code>.</p>

<p><strong>Đồ thị con</strong> là một đồ thị có các đỉnh và cạnh là tập con của đồ thị ban đầu.</p>

<p><strong>Đồ thị con liên thông</strong> là một đồ thị con mà trong đó mọi cặp đỉnh đều có thể đi tới nhau chỉ bằng các cạnh của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="Cây ví dụ 1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3700-3799/3772.Maximum%20Subgraph%20Score%20in%20a%20Tree/images/tree1fixed.png" style="width: 271px; height: 51px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], good = [1,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các node màu xanh là node tốt và các node màu đỏ là node xấu.</li>
	<li>Với mỗi node, đồ thị con liên thông tốt nhất chứa node đó là toàn bộ cây, có 2 node tốt và 1 node xấu, nên score bằng 1.</li>
	<li>Các đồ thị con liên thông khác chứa một node cũng có thể có cùng score.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="Cây ví dụ 2" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3700-3799/3772.Maximum%20Subgraph%20Score%20in%20a%20Tree/images/tree2.png" style="width: 211px; height: 231px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[1,0],[1,2],[1,3],[3,4]], good = [0,1,0,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,3,2,3,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Node 0: Đồ thị con liên thông tốt nhất gồm các node <code>0, 1, 3, 4</code>, có 3 node tốt và 1 node xấu, nên score bằng <code>3 - 1 = 2</code>.</li>
	<li>Các node 1, 3 và 4: Đồ thị con liên thông tốt nhất gồm các node <code>1, 3, 4</code>, có 3 node tốt, nên score bằng 3.</li>
	<li>Node 2: Đồ thị con liên thông tốt nhất gồm các node <code>1, 2, 3, 4</code>, có 3 node tốt và 1 node xấu, nên score bằng <code>3 - 1 = 2</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="Cây ví dụ 3" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3700-3799/3772.Maximum%20Subgraph%20Score%20in%20a%20Tree/images/tree3.png" style="width: 161px; height: 51px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1]], good = [0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mỗi node, việc thêm node còn lại chỉ làm tăng thêm một node xấu, nên score tốt nhất cho cả hai node là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>good.length == n</code></li>
	<li><code>0 &lt;= good[i] &lt;= 1</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Score của một đồ thị con liên thông chứa $i$ bằng số đỉnh tốt trừ số đỉnh xấu, nên ta chỉ giữ lại các node con có đóng góp dương. Tree DP tính score đi xuống tại mỗi gốc; sau đó rerooting gộp thêm đóng góp dương từ phía node cha để thu được maximum toàn cục cho mỗi node.

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
