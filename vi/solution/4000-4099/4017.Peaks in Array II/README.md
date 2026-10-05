---
comments: true
difficulty: Hard
rating: 2515
source: Weekly Contest 514 Q4
tags:
    - Segment Tree
    - Array
    - Divide and Conquer
---

<!-- problem:start -->

# [4017. Peaks in Array II](https://leetcode.com/problems/peaks-in-array-ii)

[中文文档](/solution/4000-4099/4017.Peaks%20in%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng số nguyên 2D <code>queries</code>.</p>

<p>Một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> <code>nums[i..j]</code> được gọi là <strong>mảng con có đỉnh</strong> nếu:</p>

<ul>
	<li>Độ dài của nó <strong>ít nhất</strong> là 3.</li>
	<li>Tồn tại một chỉ số <code>k</code> sao cho <code>i &lt; k &lt; j</code> và:
	<ul>
		<li><code>nums[k] &gt; nums[k - 1]</code></li>
		<li><code>nums[k] &gt; nums[k + 1]</code></li>
	</ul>
	</li>
</ul>

<p>Cần xử lý hai loại truy vấn:</p>

<ul>
	<li><code>[1, l<sub>i</sub>, r<sub>i</sub>]</code>: Tính số <strong>mảng con có đỉnh</strong> nằm hoàn toàn trong <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.</li>
	<li><code>[2, index<sub>i</sub>, val<sub>i</sub>]</code>: Cập nhật <code>nums[index<sub>i</sub>]</code> thành <code>val<sub>i</sub></code>. Cập nhật này được áp dụng cho tất cả truy vấn tiếp theo.</li>
</ul>

<p>Trả về một mảng <code>answer</code>, trong đó <code>answer[i]</code> là đáp án cho truy vấn loại 1 thứ <code>i<sup>th</sup></code> theo thứ tự xuất hiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2,4], queries = [[1,0,3],[2,1,1],[1,0,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,0]</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Truy vấn <code>[1, 0, 3]</code>:

    <ul>
    	<li><code>[1, 3, 2]</code>: chọn <code>k = 1</code>. Khi đó <code>nums[k] = 3</code>, <code>nums[k - 1] = 1</code> và <code>nums[k + 1] = 2</code>. Vì <code>3 &gt; 1</code> và <code>3 &gt; 2</code>, đây là một mảng con có đỉnh.</li>
    	<li><code>[1, 3, 2, 4]</code>: chọn <code>k = 1</code>. Khi đó <code>nums[k] = 3</code>, <code>nums[k - 1] = 1</code> và <code>nums[k + 1] = 2</code>. Vì <code>3 &gt; 1</code> và <code>3 &gt; 2</code>, đây là một mảng con có đỉnh.</li>
    </ul>
    </li>
    <li>Truy vấn <code>[2, 1, 1]</code>: Cập nhật <code>nums[1]</code> thành 1. Mảng trở thành <code>[1, 1, 2, 4]</code>.</li>
    <li>Truy vấn <code>[1, 0, 3]</code>: Hiện không có mảng con có đỉnh nào.</li>
    <li>Do đó, <code>answer = [2, 0]</code>.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,8,9,8], queries = [[1,1,3],[2,2,1],[1,0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Truy vấn <code>[1, 1, 3]</code>:

    <ul>
    	<li><code>nums[1..3] = [8, 9, 8]</code>: chọn <code>k = 2</code>. Khi đó <code>nums[k] = 9</code>, <code>nums[k - 1] = 8</code> và <code>nums[k + 1] = 8</code>. Vì <code>9 &gt; 8</code> và <code>9 &gt; 8</code>, đây là một mảng con có đỉnh.</li>
    </ul>
    </li>
    <li>Truy vấn <code>[2, 2, 1]</code>: Cập nhật <code>nums[2]</code> thành 1. Mảng trở thành <code>[9, 8, 1, 8]</code>.</li>
    <li>Truy vấn <code>[1, 0, 2]</code>: Hiện không có mảng con có đỉnh nào.</li>
    <li>Do đó, <code>answer = [1, 0]</code>.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,6,2,7,1], queries = [[1,1,3],[2,3,0],[1,0,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Truy vấn <code>[1, 1, 3]</code>: Mảng con duy nhất có độ dài ít nhất 3 là <code>[6, 2, 7]</code>. Chỉ số đỉnh có thể duy nhất là <code>k = 2</code>, nhưng <code>nums[2] = 2</code> nhỏ hơn cả <code>nums[1] = 6</code> và <code>nums[3] = 7</code>, nên đây không phải là mảng con có đỉnh.</li>
	<li>Truy vấn <code>[2, 3, 0]</code>: Cập nhật <code>nums[3]</code> thành 0. Mảng trở thành <code>[3, 6, 2, 0, 1]</code>.</li>
	<li>Truy vấn <code>[1, 0, 4]</code>:
	<ul>
		<li><code>[3, 6, 2]</code>: chọn <code>k = 1</code>. Khi đó <code>nums[k] = 6</code>, <code>nums[k - 1] = 3</code> và <code>nums[k + 1] = 2</code>. Vì <code>6 &gt; 3</code> và <code>6 &gt; 2</code>, đây là một mảng con có đỉnh.</li>
		<li><code>[3, 6, 2, 0]</code>: chọn <code>k = 1</code>. Khi đó <code>nums[k] = 6</code>, <code>nums[k - 1] = 3</code> và <code>nums[k + 1] = 2</code>. Vì <code>6 &gt; 3</code> và <code>6 &gt; 2</code>, đây là một mảng con có đỉnh.</li>
		<li><code>[3, 6, 2, 0, 1]</code>: chọn <code>k = 1</code>. Khi đó <code>nums[k] = 6</code>, <code>nums[k - 1] = 3</code> và <code>nums[k + 1] = 2</code>. Vì <code>6 &gt; 3</code> và <code>6 &gt; 2</code>, đây là một mảng con có đỉnh.</li>
	</ul>
	</li>
	<li>Do đó, <code>answer = [0, 3]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i] = [1, l<sub>i</sub>, r<sub>i</sub>]</code> hoặc <code>queries[i] = [2, index<sub>i</sub>, val<sub>i</sub>]</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt; r<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>0 &lt;= index<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>0 &lt;= val<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cả truy vấn và cập nhật từng điểm đều có thể lên tới $10^5$, nên không thể duyệt toàn bộ đoạn truy vấn để tìm các đỉnh.
>
> Mảng con $[i,j]$ là mảng con có đỉnh khi và chỉ khi có một đỉnh nằm trong khoảng mở $(i,j)$, vì vậy đáp án chỉ phụ thuộc vào các vị trí đỉnh bên trong đoạn. Một cập nhật từng điểm chỉ làm thay đổi trạng thái đỉnh của nhiều nhất ba chỉ số lân cận.
>
> Segment tree lưu các đỉnh và số mảng con chứa ít nhất một đỉnh có thể cập nhật ba vị trí đó, đồng thời gộp đáp án trên đoạn $[l,r]$.

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
