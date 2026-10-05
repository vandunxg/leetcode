---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Array
    - Sorting
---

<!-- problem:start -->

# [4018. Total Sum of Interaction Cost in Tree Groups II 🔒](https://leetcode.com/problems/total-sum-of-interaction-cost-in-tree-groups-ii)

[中文文档](/solution/4000-4099/4018.Total%20Sum%20of%20Interaction%20Cost%20in%20Tree%20Groups%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một cây vô hướng có gốc tại nút 0 với <code>n</code> nút được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng số nguyên 2D <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh vô hướng nối các nút <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>.</p>

<p>Đồng thời, bạn được cho một mảng số nguyên <code>group</code> có độ dài <code>n</code>, trong đó <code>group[i]</code> biểu thị nhãn nhóm được gán cho nút <code>i</code>.</p>

<ul>
	<li>Hai nút <code>u</code> và <code>v</code> thuộc cùng một nhóm khi và chỉ khi <code>group[u] == group[v]</code>.</li>
	<li><strong>Chi phí tương tác</strong> giữa hai nút là khoảng cách <strong>ngắn nhất</strong> giữa chúng trên cây.</li>
</ul>

<p>Trả về tổng chi phí tương tác trên mọi cặp chỉ số nút <code>(u, v)</code> thỏa mãn <code>0 &lt;= u &lt; v &lt; n</code> và <code>group[u] == group[v]</code>.</p>

<p>Khoảng cách <strong>ngắn nhất</strong> giữa hai nút là số cạnh trên đường đi duy nhất nối chúng trong cây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], group = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4018.Total%20Sum%20of%20Interaction%20Cost%20in%20Tree%20Groups%20II/images/screenshot-2026-05-05-at-40329am.png" style="width: 300px; height: 64px;" /></p>

<p>Tất cả các nút đều thuộc nhóm 1. Chi phí tương tác giữa các cặp nút là:</p>

<ul>
	<li>Các nút <code>[0, 1]</code>: 1</li>
	<li>Các nút <code>[1, 2]</code>: 1</li>
	<li>Các nút <code>[0, 2]</code>: 2</li>
</ul>

<p>Do đó, tổng chi phí tương tác là <code>1 + 1 + 2 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], group = [3,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4018.Total%20Sum%20of%20Interaction%20Cost%20in%20Tree%20Groups%20II/images/screenshot-2026-05-05-at-40416am.png" style="width: 300px; height: 60px;" /></p>

<ul>
	<li>Nút 0 và nút 2 thuộc nhóm 3. Chi phí tương tác giữa cặp nút này là 2.</li>
	<li>Nút 1 thuộc một nhóm khác và không tạo thành cặp hợp lệ nào. Vì vậy, tổng chi phí tương tác là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[0,2],[0,3]], group = [1,1,4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>​​​​​​​​​​​​​​<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4018.Total%20Sum%20of%20Interaction%20Cost%20in%20Tree%20Groups%20II/images/screenshot-2026-05-05-at-40819am.png" style="width: 300px; height: 199px;" /></p>

<p>Các nút thuộc cùng nhóm và chi phí tương tác tương ứng là:</p>

<ul>
	<li>Nhóm 1: Các nút <code>[0, 1]</code>: 1</li>
	<li>Nhóm 4: Các nút <code>[2, 3]</code>: 2</li>
</ul>

<p>Do đó, tổng chi phí tương tác là <code>1 + 2 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1]], group = [1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các nút đều thuộc các nhóm khác nhau và không có cặp hợp lệ nào. Vì vậy, tổng chi phí tương tác là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>group.length == n</code></li>
	<li><code>1 &lt;= group[i] &lt;= n</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Không thể tính tổng khoảng cách bằng cách duyệt mọi cặp cùng nhóm trên một cây có $n=10^5$.
>
> Đường đi trên cây là duy nhất, nên mỗi cạnh đóng góp vào đáp án bằng tích của số đỉnh cùng nhóm ở hai phía. Một lần DFS thống kê tần suất của từng nhóm trong mỗi cây con có thể cộng dồn các tích này; không cần hiện thực hóa khoảng cách theo từng cặp.
>
> Chọn $0$ làm gốc và gộp các map của cây con khi đi lên phù hợp với các nhãn nhóm trong $[1,n]$.

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
