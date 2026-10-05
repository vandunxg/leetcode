---
comments: true
difficulty: Hard
rating: 2186
source: Weekly Contest 501 Q4
tags:
    - Graph
    - Array
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3928. Minimum Cost to Buy Apples II](https://leetcode.com/problems/minimum-cost-to-buy-apples-ii)

[中文文档](/solution/3900-3999/3928.Minimum%20Cost%20to%20Buy%20Apples%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> và một mảng số nguyên <code>prices</code> có độ dài <code>n</code>, trong đó <code>prices[i]</code> là giá táo tại cửa hàng <code>i</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>roads</code>, trong đó <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>, tax<sub>i</sub>]</code> biểu diễn một con đường <strong>hai chiều</strong>:</p>

<ul>
	<li><code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> là hai cửa hàng được nối với nhau bởi con đường.</li>
	<li><code>cost<sub>i</sub></code> là chi phí đi trên con đường khi <strong>không</strong> mang theo táo.</li>
	<li><code>tax<sub>i</sub></code> là hệ số nhân áp dụng cho <code>cost<sub>i</sub></code> khi di chuyển <strong>có</strong> mang theo táo.</li>
</ul>

<p>Với mỗi cửa hàng <code>i</code>, bạn có thể chọn một trong hai cách:</p>

<ul>
	<li>Mua táo tại cửa hàng <code>i</code> với giá <code>prices[i]</code>.</li>
	<li>Di chuyển <strong>không mang theo hàng</strong> đến một cửa hàng bất kỳ <code>j</code> bằng cách sử dụng <strong>một số lượng tùy ý</strong> con đường, mua táo với giá <code>prices[j]</code>, rồi quay về cửa hàng <code>i</code> trong khi mang theo táo, trả <code>cost * tax</code> trên mỗi con đường được sử dụng cho chuyến về.</li>
</ul>

<p>Đường đi lúc đi, khi bạn không mang theo hàng, và đường đi lúc về có thể <strong>khác nhau</strong>.</p>

<p>Hãy trả về một mảng số nguyên <code>ans</code> có độ dài <code>n</code>, trong đó <code>ans[i]</code> là <strong>tổng chi phí nhỏ nhất</strong> để mua táo khi bắt đầu từ cửa hàng <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, prices = [8,3], roads = [[0,1,1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3928.Minimum%20Cost%20to%20Buy%20Apples%20II/images/screenshot-2025-08-23-at-23341-am.png" style="width: 230px; height: 85px;" /></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th>Cửa hàng <code inline="">i</code></th>
			<th><code inline="">prices[i]</code></th>
			<th>Cửa hàng <code inline="">j</code></th>
			<th><code>prices[j]</code></th>
			<th><code inline="">cost<sub>i</sub></code></th>
			<th><code inline="">tax<sub>i</sub></code></th>
			<th>Chi phí đi</th>
			<th>Chi phí về</th>
			<th>Tổng</th>
			<th>Nhỏ nhất</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>0</td>
			<td>8</td>
			<td>1</td>
			<td>3</td>
			<td>1</td>
			<td>2</td>
			<td>1</td>
			<td><code>1 * 2 = 2</code></td>
			<td><code>1 + 2 + 3 = 6</code></td>
			<td><code>min(8, 6) = 6</code></td>
		</tr>
		<tr>
			<td>1</td>
			<td>3</td>
			<td>0</td>
			<td>8</td>
			<td>1</td>
			<td>2</td>
			<td>1</td>
			<td><code>1 * 2 = 2</code></td>
			<td><code>1 + 2 + 8 = 11</code></td>
			<td><code>min(3, 11) = 3</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là <code>[6, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, prices = [9,4,6], roads = [[0,1,1,3],[1,2,4,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[8,4,6]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3928.Minimum%20Cost%20to%20Buy%20Apples%20II/images/screenshot-2025-08-23-at-23736-am.png" style="width: 346px; height: 80px;" /><strong>​​​​​​​</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th>Cửa hàng <code inline="">i</code></th>
			<th><code inline="">prices[i]</code></th>
			<th>Cửa hàng <code inline="">j</code></th>
			<th><code>prices[j]</code></th>
			<th><code inline="">cost<sub>i</sub></code></th>
			<th><code inline="">tax<sub>i</sub></code></th>
			<th>Chi phí đi</th>
			<th>Chi phí về</th>
			<th>Tổng</th>
			<th>Nhỏ nhất</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>0</td>
			<td>9</td>
			<td>1</td>
			<td>4</td>
			<td>1</td>
			<td>3</td>
			<td>1</td>
			<td><code>1 * 3 = 3</code></td>
			<td><code>1 + 3 + 4 = 8</code></td>
			<td><code>min(9, 8) = 8</code></td>
		</tr>
		<tr>
			<td>1</td>
			<td>4</td>
			<td>2</td>
			<td>6</td>
			<td>4</td>
			<td>2</td>
			<td>4</td>
			<td><code>4 * 2 = 8</code></td>
			<td><code>4 + 8 + 6 = 18</code></td>
			<td><code>min(4, 18) = 4</code></td>
		</tr>
		<tr>
			<td>2</td>
			<td>6</td>
			<td>1</td>
			<td>4</td>
			<td>4</td>
			<td>2</td>
			<td>4</td>
			<td><code>4 * 2 = 8</code></td>
			<td><code>4 + 8 + 4 = 16</code></td>
			<td><code>min(6, 16) = 6</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là <code>[8, 4, 6]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, prices = [10,11,1], roads = [[0,2,1,3],[1,2,3,4],[0,1,5,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,11,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong>​​​​​​​​​​​​​​</strong><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3928.Minimum%20Cost%20to%20Buy%20Apples%20II/images/screenshot-2025-08-23-at-24644-am.png" style="width: 250px; height: 181px;" /></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th>Cửa hàng <code inline="">i</code></th>
			<th><code inline="">prices[i]</code></th>
			<th>Cửa hàng <code inline="">j</code></th>
			<th><code>prices[j]</code></th>
			<th><code inline="">cost<sub>i</sub></code></th>
			<th><code inline="">tax<sub>i</sub></code></th>
			<th>Chi phí đi</th>
			<th>Chi phí về</th>
			<th>Tổng</th>
			<th>Nhỏ nhất</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>0</td>
			<td>10</td>
			<td>2</td>
			<td>1</td>
			<td>1</td>
			<td>3</td>
			<td>1</td>
			<td><code>1 * 3 = 3</code></td>
			<td><code>1 + 3 + 1 = 5</code></td>
			<td><code>min(10, 5) = 5</code></td>
		</tr>
		<tr>
			<td>1</td>
			<td>11</td>
			<td>2</td>
			<td>1</td>
			<td>3</td>
			<td>4</td>
			<td>3</td>
			<td><code>3 * 4 = 12</code></td>
			<td><code>3 + 12 + 1 = 16</code></td>
			<td><code>min(11, 16) = 11</code></td>
		</tr>
		<tr>
			<td>2</td>
			<td>1</td>
			<td>0</td>
			<td>10</td>
			<td>1</td>
			<td>3</td>
			<td>1</td>
			<td><code>1 * 3 = 3</code></td>
			<td><code>1 + 3 + 10 = 14</code></td>
			<td><code>min(1, 14) = 1</code></td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là <code>[5, 11, 1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>prices.length == n</code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= roads.length &lt;= min(n &times; (n - 1) / 2, 2000)</code></li>
	<li><code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>, tax<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li><code>1 &lt;= cost<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>​​​​​​​1 &lt;= tax<sub>​​​​​​​i</sub> &lt;= 100</code>​​​​​​​</li>
	<li>Không có các cạnh <strong>trùng lặp</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thực hiện một lần tìm đường đi ngắn nhất riêng từ mỗi cửa hàng đến mọi cửa hàng khác có độ phức tạp khoảng $O(n^2\log n)$, vốn đã sát với giới hạn khi $n\le 1000$. Chuyến đi lúc đi không mang theo hàng, còn chuyến về nhân chi phí với $\textit{tax}$, nên hai chiều không thể dùng chung một bảng khoảng cách.
>
> Hãy tính riêng khoảng cách khi không mang theo hàng và khoảng cách khi đang mang hàng, trong đó khoảng cách sau là đường đi ngắn nhất trên các cạnh có trọng số $cost\cdot tax$. Với cửa hàng $i$, đáp án là $\min_j(\mathrm{dist}_{\mathrm{empty}}(i,j)+\textit{prices}[j]+\mathrm{dist}_{\mathrm{load}}(j,i))$.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng lại ở phép quy giản thành hai loại trọng số đó.

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
