---
comments: true
difficulty: Hard
rating: 2748
source: Weekly Contest 436 Q4
tags:
    - Greedy
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3449. Maximize the Minimum Game Score](https://leetcode.com/problems/maximize-the-minimum-game-score)

[中文文档](/solution/3400-3499/3449.Maximize%20the%20Minimum%20Game%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>points</code> có kích thước <code>n</code> và một số nguyên <code>m</code>. Có một mảng khác <code>gameScore</code> có kích thước <code>n</code>, trong đó <code>gameScore[i]</code> biểu thị điểm đạt được ở trò chơi thứ <code>i<sup>th</sup></code>. Ban đầu, <code>gameScore[i] == 0</code> với mọi <code>i</code>.</p>

<p>Bạn bắt đầu tại chỉ số -1, nằm ngoài mảng (ở trước vị trí đầu tiên có chỉ số 0). Bạn có thể thực hiện <strong>nhiều nhất</strong> <code>m</code> bước. Ở mỗi bước, bạn có thể:</p>

<ul>
	<li>Tăng chỉ số lên 1 và cộng <code>points[i]</code> vào <code>gameScore[i]</code>.</li>
	<li>Giảm chỉ số đi 1 và cộng <code>points[i]</code> vào <code>gameScore[i]</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng sau bước đầu tiên, chỉ số luôn phải nằm trong phạm vi của mảng.</p>

<p>Trả về giá trị <strong>nhỏ nhất lớn nhất có thể</strong> của <code>gameScore</code> sau <strong>nhiều nhất</strong> <code>m</code> bước.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [2,4], m = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, chỉ số <code>i = -1</code> và <code>gameScore = [0, 0]</code>.</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Bước</th>
			<th style="border: 1px solid black;">Chỉ số</th>
			<th style="border: 1px solid black;">gameScore</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">Tăng <code>i</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[2, 0]</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Tăng <code>i</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[2, 4]</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Giảm <code>i</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[4, 4]</code></td>
		</tr>
	</tbody>
</table>

<p>Giá trị nhỏ nhất trong <code>gameScore</code> là 4, và đây là giá trị nhỏ nhất lớn nhất có thể đạt được trong mọi cấu hình. Do đó, đầu ra là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [1,2,3], m = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, chỉ số <code>i = -1</code> và <code>gameScore = [0, 0, 0]</code>.</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Bước</th>
			<th style="border: 1px solid black;">Chỉ số</th>
			<th style="border: 1px solid black;">gameScore</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">Tăng <code>i</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[1, 0, 0]</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Tăng <code>i</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[1, 2, 0]</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Giảm <code>i</code></td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>[2, 2, 0]</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Tăng <code>i</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>[2, 4, 0]</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Tăng <code>i</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>[2, 4, 3]</code></td>
		</tr>
	</tbody>
</table>

<p>Giá trị nhỏ nhất trong <code>gameScore</code> là 2, và đây là giá trị nhỏ nhất lớn nhất có thể đạt được trong mọi cấu hình. Do đó, đầu ra là 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == points.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= points[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta di chuyển qua lại trên một đường thẳng trong $m$ bước; mỗi lần đi qua $i$ sẽ cộng thêm $\textit{points}[i]$. Mục tiêu là tối đa hóa điểm nhỏ nhất. Vì $m$ có thể lên đến $10^9$, ta không thể mô phỏng từng bước.
>
> Tối đa hóa giá trị nhỏ nhất dẫn đến việc tìm kiếm nhị phân. Với một giá trị $x$, điều kiện khả thi là chỉ số $i$ được đi qua ít nhất $\lceil x/\textit{points}[i]\rceil$ lần.
>
> Greedy từ trái sang phải sẽ xử lý các lượt đi còn thiếu bằng cách đi đến điểm đó rồi quay lại, cộng dồn số bước phát sinh, sau đó so sánh với $m$. Giá trị $x$ khả thi nhỏ nhất chính là đáp án.

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
