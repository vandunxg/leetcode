---
comments: true
difficulty: Medium
rating: 1840
source: Weekly Contest 507 Q3
tags:
    - Graph
    - String
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3970. Shortest Path With At Most K Consecutive Identical Characters](https://leetcode.com/problems/shortest-path-with-at-most-k-consecutive-identical-characters)

[中文文档](/solution/3900-3999/3970.Shortest%20Path%20With%20At%20Most%20K%20Consecutive%20Identical%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số lượng node trong một đồ thị <strong>có hướng và có trọng số</strong>, được đánh số từ 0 đến <code>n - 1</code>. Đồ thị được biểu diễn bằng mảng số nguyên hai chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> biểu diễn một cạnh có hướng từ node <code>u<sub>i</sub></code> đến node <code>v<sub>i</sub></code> với trọng số <code>w<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một chuỗi <code>labels</code> có độ dài <code>n</code>, trong đó <code>labels[i]</code> là ký tự được gán cho node <code>i</code>, và một số nguyên <code>k</code>.</p>

<p>Hãy trả về <strong>tổng</strong> trọng số cạnh <strong>nhỏ nhất</strong> của một đường đi từ node 0 đến node <code>n - 1</code> sao cho phép nối các nhãn của các node trên đường đi chứa <strong>không quá</strong> <code>k</code> ký tự <strong>liên tiếp</strong> <strong>giống nhau</strong>. Nếu không tồn tại đường đi hợp lệ, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,1],[1,2,1],[0,2,3]], labels = &quot;aab&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi hợp lệ tối ưu từ node 0 đến node 2 như sau:</p>

<ul>
	<li>Sử dụng <code>edges[2] = [0, 2, 3]</code> để đến node 2 với trọng số <code>w<sub>i</sub> = 3</code>.</li>
</ul>
Phép nối các nhãn tương ứng là <code>&quot;ab&quot;</code>, thỏa mãn điều kiện có không quá <code>k = 1</code> ký tự giống nhau liên tiếp. Do đó, đáp án là 3.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,1],[1,2,1],[0,2,3]], labels = &quot;aab&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi hợp lệ tối ưu từ node 0 đến node 2 như sau:</p>

<ul>
	<li>Sử dụng <code>edges[0] = [0, 1, 1]</code> để đến node 1 với trọng số <code>w<sub>i</sub> = 1</code>.</li>
	<li>Sử dụng <code>edges[1] = [1, 2, 1]</code> để đến node 2 với trọng số <code>w<sub>i</sub> = 1</code>.</li>
</ul>
Phép nối các nhãn tương ứng là <code>&quot;aab&quot;</code>, thỏa mãn điều kiện có không quá <code>k = 2</code> ký tự giống nhau liên tiếp. Do đó, đáp án là 2.</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,1],[1,2,1]], labels = &quot;aaa&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại đường đi hợp lệ từ node 0 đến node 2 thỏa mãn điều kiện có không quá <code>k = 2</code> ký tự giống nhau liên tiếp. Do đó, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == labels.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li><code>labels</code> chỉ gồm các chữ cái tiếng Anh viết thường</li>
	<li><code>1 &lt;= k &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi ngắn nhất thông thường bỏ qua nhãn, nên có thể tạo ra hơn $k$ ký tự giống nhau liên tiếp. Trạng thái phải ghi nhớ độ dài đoạn liên tiếp hiện tại.
>
> Chạy Dijkstra trên $(\textit{node},\textit{run})$ với điều kiện $\textit{run}\le k$: tăng độ dài đoạn khi nhãn kế tiếp trùng khớp và đặt lại về giá trị ban đầu khi không trùng. Tích của $n$ và $k$ là cận trên của không gian trạng thái.
>
> Thư mục này chưa có lời giải được cài đặt; phần trình bày dừng ở ý tưởng đường đi ngắn nhất với trạng thái mở rộng này.

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
