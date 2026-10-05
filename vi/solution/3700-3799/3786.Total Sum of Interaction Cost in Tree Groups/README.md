---
comments: true
difficulty: Hard
rating: 2139
source: Weekly Contest 481 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
---

<!-- problem:start -->

# [3786. Total Sum of Interaction Cost in Tree Groups](https://leetcode.com/problems/total-sum-of-interaction-cost-in-tree-groups)

[中文文档](/solution/3700-3799/3786.Total%20Sum%20of%20Interaction%20Cost%20in%20Tree%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> và một cây vô hướng gồm <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh vô hướng nối node <code>u<sub>i</sub></code> với node <code>v<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>group</code> có độ dài <code>n</code>, trong đó <code>group[i]</code> biểu thị nhãn nhóm được gán cho node <code>i</code>.</p>

<ul>
	<li>Hai node <code>u</code> và <code>v</code> được xem là thuộc cùng một nhóm nếu <code>group[u] == group[v]</code>.</li>
	<li><strong>Chi phí tương tác</strong> giữa <code>u</code> và <code>v</code> được định nghĩa là số cạnh trên đường đi duy nhất nối chúng trong cây.</li>
</ul>

<p>Trả về một số nguyên biểu thị <strong>tổng</strong> chi phí tương tác trên tất cả các cặp <strong>không có thứ tự</strong> <code>(u, v)</code> với <code>u != v</code> sao cho <code>group[u] == group[v]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], group = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3700-3799/3786.Total%20Sum%20of%20Interaction%20Cost%20in%20Tree%20Groups/images/screenshot-2025-09-24-at-50538-pm.png" style="width: 250px; height: 57px;" /></strong></p>

<p>Tất cả node đều thuộc nhóm 1. Chi phí tương tác giữa các cặp node là:</p>

<ul>
	<li>Các node <code>(0, 1)</code>: 1</li>
	<li>Các node <code>(1, 2)</code>: 1</li>
	<li>Các node <code>(0, 2)</code>: 2</li>
</ul>

<p>Vậy tổng chi phí tương tác là <code>1 + 1 + 2 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], group = [3,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Node 0 và node 2 thuộc nhóm 3. Chi phí tương tác giữa cặp node này là 2.</li>
	<li>Node 1 thuộc một nhóm khác và không tạo thành cặp hợp lệ nào. Do đó, tổng chi phí tương tác là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[0,2],[0,3]], group = [1,1,4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3700-3799/3786.Total%20Sum%20of%20Interaction%20Cost%20in%20Tree%20Groups/images/screenshot-2025-09-24-at-51312-pm.png" style="width: 200px; height: 146px;" /></p>

<p>Các node thuộc cùng nhóm và chi phí tương tác tương ứng là:</p>

<ul>
	<li>Nhóm 1: Các node <code>(0, 1)</code>: 1</li>
	<li>Nhóm 4: Các node <code>(2, 3)</code>: 2</li>
</ul>

<p>Vậy tổng chi phí tương tác là <code>1 + 2 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1]], group = [9,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả node đều thuộc các nhóm khác nhau và không có cặp hợp lệ nào. Do đó, tổng chi phí tương tác là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>group.length == n</code></li>
	<li><code>1 &lt;= group[i] &lt;= 20</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính tổng độ dài đường đi trên các cặp không có thứ tự cùng nhóm, và có nhiều nhất $20$ nhóm. Khoảng cách giữa hai node bằng tổng độ sâu trừ đi hai lần độ sâu của LCA. Tree DP theo từng nhóm, đếm số node cùng nhóm nằm bên trong và bên ngoài mỗi cây con, sẽ cộng dồn khoảng cách giữa các node cùng nhóm trong thời gian gần tuyến tính.

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
