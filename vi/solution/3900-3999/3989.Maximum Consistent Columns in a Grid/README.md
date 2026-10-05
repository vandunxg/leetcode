---
comments: true
difficulty: Hard
rating: 2013
source: Weekly Contest 510 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3989. Maximum Consistent Columns in a Grid](https://leetcode.com/problems/maximum-consistent-columns-in-a-grid)

[中文文档](/solution/3900-3999/3989.Maximum%20Consistent%20Columns%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>grid</code> có kích thước <code>m x n</code> và một số nguyên <code>limit</code>.</p>

<p>Bạn có thể xóa không hoặc nhiều cột khỏi grid, nhưng phải giữ lại ít nhất một cột. <strong>Thứ tự tương đối</strong> của các cột còn lại phải được bảo toàn.</p>

<p>Một lưới được gọi là <strong>nhất quán</strong> nếu với mọi hàng <code>i</code> và mọi cặp cột còn lại kề nhau <code>a</code> và <code>b</code> với <code>a &lt; b</code>, điều kiện sau được thỏa mãn: <code>|grid[i][b] - grid[i][a]| &lt;= limit</code>.</p>

<p>Trả về số lượng <strong>lớn nhất</strong> các cột có thể giữ lại sao cho lưới kết quả là <strong>nhất quán</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[-2,0,3]], limit = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa cột 2 và giữ lại các cột 0 và 1, khi đó <code>|grid[0][1] &minus; grid[0][0]| = |0 &minus; (&minus;2)| = 2 &lt;= limit</code>.</li>
	<li>Vì vậy, số cột lớn nhất có thể giữ lại là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,-1,1],[2,2,2]], limit = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa cột 1 và giữ lại các cột 0 và 2, khi đó
	<ul>
		<li><code>|grid[0][2] &minus; grid[0][0]| = |1 &minus; 1| = 0 &lt;= limit</code> và</li>
		<li><code>|grid[1][2] &minus; grid[1][0]| = |2 &minus; 2| = 0 &lt;= limit</code>.</li>
	</ul>
	</li>
	<li>Vì vậy, số cột lớn nhất có thể giữ lại là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[-5,5]], limit = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa một trong hai cột 0 hoặc 1, vì <code>|grid[0][1] &minus; grid[0][0]| = |5 &minus; (&minus;5)| = 10 &gt; limit</code>.</li>
	<li>Vì vậy, số cột lớn nhất có thể giữ lại là 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m == grid.length &lt;= 250</code></li>
	<li><code>1 &lt;= n == grid[i].length &lt;= 250</code></li>
	<li><code>-10<sup>5</sup> &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= limit &lt;= 10<sup>5</sup>​​​​​​​​​​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi xóa, mọi cặp cột kề nhau được giữ lại phải có độ chênh lệch không quá $\textit{limit}$ trên mọi hàng. Thứ tự được cố định, nên ta cần tìm dãy con cột dài nhất thỏa mãn điều kiện giữa các cột kề nhau.
>
> Khi số cột ở mức vừa phải, ta có thể dùng quy hoạch động kiểu LIS: $\textit{dp}[j]$ là độ dài dãy nhất quán dài nhất kết thúc tại $j$, và chuyển $i\to j$ chỉ hợp lệ khi mọi hàng đều thỏa mãn giới hạn.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng ở quy hoạch động trên các cột.

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
