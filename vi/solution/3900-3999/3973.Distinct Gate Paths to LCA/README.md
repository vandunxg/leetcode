---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Array
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3973. Distinct Gate Paths to LCA 🔒](https://leetcode.com/problems/distinct-gate-paths-to-lca)

[中文文档](/solution/3900-3999/3973.Distinct%20Gate%20Paths%20to%20LCA/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một cây vô hướng có gốc là node 0, gồm <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>, được biểu diễn bằng mảng <code>parent</code>, trong đó <code>parent[i]</code> là node cha của node <code>i</code>.</p>

<p>Mỗi node <code>i</code> có ba loại cổng, được cho bởi mảng hai chiều <code>gates</code>, trong đó <code>gates[i] = [red<sub>i</sub>, blue<sub>i</sub>, white<sub>i</sub>]</code> biểu thị số lượng cổng <strong>đỏ</strong>, <strong>xanh dương</strong> và <strong>trắng</strong> tại node <code>i</code>.</p>

<ul>
	<li>Cổng <strong>đỏ</strong>: chỉ có thể sử dụng với thẻ <strong>đỏ</strong>.</li>
	<li>Cổng <strong>xanh dương</strong>: chỉ có thể sử dụng với thẻ <strong>xanh dương</strong>.</li>
	<li>Cổng <strong>trắng</strong>: có thể sử dụng với <strong>một trong hai</strong> thẻ, nhưng sẽ <strong>đổi</strong> màu thẻ khi sử dụng.</li>
</ul>

<p>Alice và Bob bắt đầu tại các node cho trước với một thẻ đỏ hoặc xanh dương (<code>1</code> = đỏ, <code>0</code> = xanh dương). Họ phải <strong>độc lập</strong> di chuyển <strong>lên trên</strong> đến <strong>tổ tiên chung gần nhất (LCA)</strong> của mình.</p>

<p>Tại mỗi node, một người chỉ có thể di chuyển đến node cha <strong>khi</strong> có thể sử dụng <strong>ít nhất</strong> một cổng tại node đó bằng thẻ hiện tại. Có thể sử dụng cổng <strong>trắng</strong> tùy ý số lần để đổi màu thẻ.</p>

<p><strong>Quy tắc di chuyển (một bước di chuyển = từ <code>u</code> đến <code>parent[u]</code>):</strong></p>

<ul>
	<li>Chỉ được di chuyển lên trên về phía gốc.</li>
	<li>Tại node <code>u</code>, chọn <strong>chính xác</strong> một thể hiện cổng cụ thể. Các cổng giống nhau được xem là <strong>riêng biệt</strong> và được đếm riêng.</li>
	<li>Nếu đang giữ thẻ <strong>đỏ</strong>: sử dụng cổng đỏ để tiếp tục giữ thẻ đỏ, hoặc cổng trắng để <strong>đổi</strong> thành thẻ xanh dương.</li>
	<li>Nếu đang giữ thẻ <strong>xanh dương</strong>: sử dụng cổng xanh dương để tiếp tục giữ thẻ xanh dương, hoặc cổng trắng để <strong>đổi</strong> thành thẻ đỏ.</li>
	<li>Nếu tại <code>u</code> không có cổng nào có thể sử dụng, chuỗi di chuyển kết thúc.</li>
</ul>

<p>Bạn cũng được cho một mảng hai chiều <code>queries</code>, trong đó <code>queries[i] = [aNode<sub>i</sub>, aCard<sub>i</sub>, bNode<sub>i</sub>, bCard<sub>i</sub>]</code>:</p>

<ul>
	<li><code>aNode<sub>i</sub></code>, <code>aCard<sub>i</sub></code>: node bắt đầu và thẻ ban đầu của Alice.</li>
	<li><code>bNode<sub>i</sub></code>, <code>bCard<sub>i</sub></code>: node bắt đầu và thẻ ban đầu của Bob.</li>
</ul>

<p>Với mỗi truy vấn, hãy đếm số cách hợp lệ <strong>khác nhau</strong> theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code> để cả hai người đi đến <strong>LCA</strong> của họ.</p>

<p>Sau khi tính kết quả cho tất cả truy vấn, trả về giá trị <strong>xor theo bit</strong> của các kết quả đó.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Hai cách được xem là khác nhau nếu tập hợp các cổng Alice hoặc Bob sử dụng <strong>khác nhau</strong>.</li>
	<li>Nếu một người đã ở tại <strong>LCA</strong>, số cách của người đó là 1.</li>
	<li><strong>Tổ tiên chung gần nhất (LCA)</strong> của hai node <code>a</code> và <code>b</code> là node thấp nhất trong cây có cả <code>a</code> và <code>b</code> là hậu duệ (một node được phép là hậu duệ của chính nó).</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, parent = [-1,0,0], gates = [[1,0,1],[0,1,1],[1,1,0]], queries = [[1,0,2,0],[1,1,2,0],[1,0,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th align="center"><code>i</code></th>
			<th align="center">Alice<br />
			[node, thẻ]</th>
			<th align="center">Bob<br />
			[node, thẻ]</th>
			<th align="center">LCA</th>
			<th align="center">Alice<br />
			Đường đi</th>
			<th align="center">Bob<br />
			Đường đi</th>
			<th align="center">Số cách của Alice</th>
			<th align="center">Số cách của Bob</th>
			<th align="center">Tổng số cách</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center">0</td>
			<td align="center">[1, 0]: Thẻ xanh dương</td>
			<td align="center">[2, 0]: Thẻ xanh dương</td>
			<td align="center">0</td>
			<td align="center">1 &rarr; 0</td>
			<td align="center">2 &rarr; 0</td>
			<td align="center">2 (1 cổng xanh dương + 1 cổng trắng tại node 1)</td>
			<td align="center">1 (1 cổng xanh dương tại node 2)</td>
			<td align="center">2 &times; 1 = 2</td>
		</tr>
		<tr>
			<td align="center">1</td>
			<td align="center">[1, 1]: Thẻ đỏ</td>
			<td align="center">[2, 0]: Thẻ xanh dương</td>
			<td align="center">0</td>
			<td align="center">1 &rarr; 0</td>
			<td align="center">2 &rarr; 0</td>
			<td align="center">1 (1 cổng trắng tại node 1)</td>
			<td align="center">1 (1 cổng xanh dương tại node 2)</td>
			<td align="center">1 &times; 1 = 1</td>
		</tr>
		<tr>
			<td align="center">2</td>
			<td align="center">[1, 0]: Thẻ xanh dương</td>
			<td align="center">[2, 1]: Thẻ đỏ</td>
			<td align="center">0</td>
			<td align="center">1 &rarr; 0</td>
			<td align="center">2 &rarr; 0</td>
			<td align="center">2 (1 cổng xanh dương + 1 cổng trắng tại node 1)</td>
			<td align="center">1 (1 cổng đỏ tại node 2)</td>
			<td align="center">2 &times; 1 = 2</td>
		</tr>
	</tbody>
</table>

<p>Do đó, XOR của tất cả giá trị là: <code>2 XOR 1 XOR 2 = 1</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, parent = [-1,0,1], gates = [[0,1,2],[1,0,1],[0,0,3]], queries = [[2,0,1,0],[2,1,0,0],[1,1,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<div class="example-block">
<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th align="center"><code>i</code></th>
			<th align="center">Alice<br />
			[node, thẻ]</th>
			<th align="center">Bob<br />
			[node, thẻ]</th>
			<th align="center">LCA</th>
			<th align="center">Đường đi của Alice</th>
			<th align="center">Đường đi của Bob</th>
			<th align="center">Số cách của Alice</th>
			<th align="center">Số cách của Bob</th>
			<th align="center">Tổng số cách</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center">0</td>
			<td align="center">[2, 0]: Thẻ xanh dương</td>
			<td align="center">[1, 0]: Thẻ xanh dương</td>
			<td align="center">1</td>
			<td align="center">2 &rarr; 1</td>
			<td align="center">1</td>
			<td align="center">3 (3 cổng trắng tại node 2)</td>
			<td align="center">1 (không di chuyển)</td>
			<td align="center">3 &times; 1 = 3</td>
		</tr>
		<tr>
			<td align="center">1</td>
			<td align="center">[2, 1]: Thẻ đỏ</td>
			<td align="center">[0, 0]: Thẻ xanh dương</td>
			<td align="center">0</td>
			<td align="center">2 &rarr; 1 &rarr; 0</td>
			<td align="center">0</td>
			<td align="center">3 (3 cổng trắng tại node 2) &times; 1 (1 cổng trắng tại node 1) = 3</td>
			<td align="center">1 (không di chuyển)</td>
			<td align="center">3 &times; 1 = 3</td>
		</tr>
		<tr>
			<td align="center">2</td>
			<td align="center">[1, 1]: Thẻ đỏ</td>
			<td align="center">[2, 1]: Thẻ đỏ</td>
			<td align="center">1</td>
			<td align="center">1</td>
			<td align="center">2 &rarr; 1</td>
			<td align="center">1 (không di chuyển)</td>
			<td align="center">3 (3 cổng trắng tại node 2)</td>
			<td align="center">1 &times; 3 = 3</td>
		</tr>
	</tbody>
</table>

<p>Do đó, XOR của tất cả giá trị là: <code>3 XOR 3 XOR 3 = 3</code>.</p>
</div>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong>​​​​​​​</p>

<ul>
	<li><code>2 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>n == parent.length == gates.length</code></li>
	<li><code>parent[0] == -1</code></li>
	<li><code>0 &lt;= parent[i] &lt; n</code> với <code>i</code> thuộc <code>[1, n - 1]</code></li>
	<li><code>gates[i] == [red<sub>i</sub>, blue<sub>i</sub>, white<sub>i</sub>]</code></li>
	<li><code>0 &lt;= red<sub>i</sub>, blue<sub>i</sub>, white<sub>i</sub> &lt;= 10</code></li>
	<li><code>1 &lt;= queries.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>queries[i] = [aNode<sub>i</sub>, aCard<sub>i</sub>, bNode<sub>i</sub>, bCard<sub>i</sub>]</code></li>
	<li><code>0 &lt;= aNode<sub>i</sub>, bNode<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>0 &lt;= aCard<sub>i</sub>, bCard<sub>i</sub> &lt;= 1</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho mảng <code>parent</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người độc lập đi lên LCA, tiêu tốn một cổng có thể sử dụng ở mỗi bước; cổng trắng sẽ đổi màu thẻ. Có nhiều truy vấn trên cùng một cây nên không thể mô phỏng từng bước cho mỗi truy vấn.
>
> Với mỗi node, tiền xử lý khoảng cách mà thẻ đỏ hoặc xanh dương có thể đi lên và số chuỗi cổng tương ứng, bằng cách gộp số lượng trên bảng binary lifting. Mỗi truy vấn nhân số cách đi trên đường của Alice và Bob.
>
> Thư mục này chưa có lời giải được cài đặt; phần trình bày dừng ở việc gộp các tổ hợp cổng bằng lifting.

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
