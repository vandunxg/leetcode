---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3949. Subtree Inversion Sum II 🔒](https://leetcode.com/problems/subtree-inversion-sum-ii)

[中文文档](/solution/3900-3999/3949.Subtree%20Inversion%20Sum%20II/README.md)

## Mô tả

<!-- description:start -->

<p data-end="551" data-start="302">Cho một cây vô hướng có gốc tại node 0, gồm <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu diễn một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code>.</p>

<p data-end="670" data-start="553">Ngoài ra, cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums[i]</code> biểu diễn giá trị tại node <code>i</code>, và một số nguyên <code>k</code>.</p>

<p data-end="763" data-start="672">Bạn có thể thực hiện các <strong>thao tác đảo dấu</strong> trên một <span data-keyword="subset">tập con</span> các node, tuân theo những quy tắc sau:</p>

<ul data-end="1247" data-start="765">
	<li data-end="890" data-start="765">
	<p data-end="799" data-start="767"><strong data-end="799" data-start="767">Thao tác đảo dấu cây con:</strong></p>

    <ul data-end="890" data-start="802">
     	<li data-end="887" data-start="802">
     	<p data-end="887" data-start="804">Khi đảo dấu một node, mọi giá trị trong <span data-keyword="subtree-of-node">cây con</span> có gốc tại node đó được nhân với -1.</p>
     	</li>
    </ul>
    </li>
    <li data-end="1247" data-start="891">
    <p data-end="931" data-start="893"><strong data-end="931" data-start="893">Ràng buộc khoảng cách giữa các lần đảo dấu:</strong></p>

    <ul data-end="1247" data-start="934">
     	<li data-end="1020" data-start="934">
     	<p data-end="1020" data-start="936">Bạn chỉ có thể đảo dấu một node nếu nó ở “đủ xa” so với mọi node khác đã được đảo dấu.</p>
     	</li>
     	<li data-end="1247" data-start="1023">
     	<p data-end="1247" data-start="1025">Nếu đảo dấu hai node <code>a</code> và <code>b</code>, <strong>khoảng cách</strong> (số cạnh trên đường đi duy nhất giữa chúng) phải <strong>ít nhất bằng</strong> <code>k</code>.</p>
     	</li>
    </ul>
    </li>

</ul>

<p data-end="1358" data-start="1249">Trả về <strong>tổng</strong> <strong>lớn nhất</strong> có thể của các giá trị tại node trong cây sau khi thực hiện các <strong>thao tác đảo dấu</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2],[0,3],[1,4],[1,5]], nums = [1,0,-10,3,4,5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">23</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3949.Subtree%20Inversion%20Sum%20II/images/4183example1drawio.png" style="width: 602px; height: 221px;" /></p>

<p>Sau khi đảo dấu cây con có gốc tại node 2, tổng lớn nhất trở thành <code>1 + 0 + 10 + 3 + 4 + 5 = 23</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[1,2]], nums = [5,-10,-10], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3949.Subtree%20Inversion%20Sum%20II/images/4183example2drawio.png" style="width: 531px; height: 63px;" /></strong></p>

<p>Sau khi đảo dấu cây con có gốc tại node 1, tổng lớn nhất trở thành <code>5 + 10 + 10 = 25</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2]], nums = [1,-5,-6], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3949.Subtree%20Inversion%20Sum%20II/images/4183example3drawio.png" style="width: 461px; height: 141px;" /></p>

<ul>
	<li>Sau khi đảo dấu các cây con có gốc tại node 1 và node 2, <code>nums = [1, 5, 6]</code>.</li>
	<li>Điều này hợp lệ vì node 1 và node 2 cách nhau hai cạnh (<code>1 &rarr; 0</code> và <code>0 &rarr; 2</code>), thỏa mãn ít nhất bằng <code>k</code>.</li>
	<li>Tổng lớn nhất là <code>1 + 5 + 6 = 12</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2]], nums = [1,-5,-6], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3949.Subtree%20Inversion%20Sum%20II/images/4183example4drawio.png" style="width: 461px; height: 142px;" /></p>

<ul>
	<li>Sau khi đảo dấu cây con có gốc tại node 0, <code>nums = [-1, 5, 6]</code>.</li>
	<li>Tổng lớn nhất là <code>(-1) + 5 + 6 = 10</code>.</li>
	<li>Lưu ý rằng ta không thể đảo dấu node 1 và node 2 vì khoảng cách giữa chúng là <code>2 &lt; k = 3</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == n</code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= edges[i][0], edges[i][1] &lt; n</code></li>
	<li><code>-4 * 10<sup>4</sup> &lt;= nums[i] &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= 50</code></li>
	<li>Đảm bảo rằng <code>edges</code> tạo thành một cây.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 5\times 10^4$ và $k\le 50$, nên không thể liệt kê các tập con đảo dấu. Một phép đảo dấu nhân cả cây con với $-1$; các phép đảo dấu chồng lấp sẽ triệt tiêu nhau, còn khoảng cách $\ge k$ ngăn không cho hai phép đảo dấu nằm quá gần nhau trên một đường đi.
>
> Quy hoạch động trên cây tại mỗi node lưu tổng hậu tố tốt nhất khi còn $t$ bước nữa mới được phép thực hiện phép đảo dấu tiếp theo. Cạnh nối với node cha làm giảm thời gian chờ này; không thể thực hiện phép đảo dấu mới khi thời gian chờ vẫn dương.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng ở quy hoạch động với thời gian chờ đó.

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
