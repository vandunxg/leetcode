---
comments: true
difficulty: Hard
rating: 2128
source: Weekly Contest 495 Q4
---

<!-- problem:start -->

# [3887. Incremental Even-Weighted Cycle Queries](https://leetcode.com/problems/incremental-even-weighted-cycle-queries)

[中文文档](/solution/3800-3899/3887.Incremental%20Even-Weighted%20Cycle%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>.</p>

<p>Có một đồ thị <strong>vô hướng</strong> gồm <code>n</code> đỉnh được đánh số từ 0 đến <code>n - 1</code>. Ban đầu, đồ thị không có cạnh nào.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> biểu diễn một cạnh nối các đỉnh <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>, có trọng số <code>w<sub>i</sub></code>. Trọng số <code>w<sub>i</sub></code> là 0 hoặc 1.</p>

<p>Xử lý các cạnh trong <code>edges</code> theo thứ tự đã cho. Với mỗi cạnh, chỉ thêm cạnh đó vào đồ thị nếu sau khi thêm, tổng trọng số của các cạnh trong <strong>mọi</strong> chu trình của đồ thị kết quả đều <strong>chẵn</strong>.</p>

<p>Trả về một số nguyên biểu thị số cạnh đã được thêm thành công vào đồ thị.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,1],[1,2,1],[0,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3887.Incremental%20Even-Weighted%20Cycle%20Queries/images/hmadizgovu.png" style="width: 168px; height: 150px;" /></p>

<ul>
	<li><code>[0, 1, 1]</code>: Thêm cạnh nối đỉnh 0 và đỉnh 1, có trọng số 1.</li>
	<li><code>[1, 2, 1]</code>: Thêm cạnh nối đỉnh 1 và đỉnh 2, có trọng số 1.</li>
	<li><code>[0, 2, 1]</code>: Không thêm cạnh nối đỉnh 0 và đỉnh 2 (cạnh nét đứt trong hình), vì chu trình <code>0 - 1 - 2 - 0</code> có tổng trọng số các cạnh là <code>1 + 1 + 1 = 3</code>, là một số lẻ.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,1],[1,2,1],[0,2,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3887.Incremental%20Even-Weighted%20Cycle%20Queries/images/rbdgrefwok.png" style="width: 179px; height: 160px;" /></p>

<ul>
	<li><code>[0, 1, 1]</code>: Thêm cạnh nối đỉnh 0 và đỉnh 1, có trọng số 1.</li>
	<li><code>[1, 2, 1]</code>: Thêm cạnh nối đỉnh 1 và đỉnh 2, có trọng số 1.</li>
	<li><code>[0, 2, 0]</code>: Thêm cạnh nối đỉnh 0 và đỉnh 2, có trọng số 0.</li>
	<li>Lưu ý rằng chu trình <code>0 - 1 - 2 - 0</code> có tổng trọng số các cạnh là <code>1 + 1 + 0 = 2</code>, là một số chẵn.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= edges.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub> &lt; v<sub>i</sub> &lt; n</code></li>
	<li>Tất cả các cạnh đều khác nhau.</li>
	<li><code>w<sub>i</sub> = 0 or w<sub>i</sub> = 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thêm các cạnh theo thứ tự, chỉ giữ lại một cạnh nếu tổng trọng số của mọi chu trình vẫn chẵn. Các trọng số là $0/1$ và $n \le 5 \times 10^4$.
>
> Điều kiện tổng trọng số của mọi chu trình đều chẵn tương đương với việc tô đồ thị bằng $2$ màu sao cho mỗi cạnh có trọng số $1$ nối hai đỉnh khác màu.
>
> Union-find với parity lưu XOR từ mỗi đỉnh đến gốc. Nếu hai đầu mút đã được nối với nhau, XOR trên đường đi phải khớp với trọng số mới; nếu không, một chu trình có tổng trọng số lẻ sẽ xuất hiện.
>
> Nếu không, hợp nhất hai tập hợp. Đếm số cạnh được giữ lại.

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
