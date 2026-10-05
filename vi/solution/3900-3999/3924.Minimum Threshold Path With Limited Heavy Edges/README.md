---
comments: true
difficulty: Hard
rating: 2079
source: Biweekly Contest 182 Q4
---

<!-- problem:start -->

# [3924. Minimum Threshold Path With Limited Heavy Edges](https://leetcode.com/problems/minimum-threshold-path-with-limited-heavy-edges)

[中文文档](/solution/3900-3999/3924.Minimum%20Threshold%20Path%20With%20Limited%20Heavy%20Edges/README.md)

## Mô tả

<!-- description:start -->
<p>Có một đồ thị vô hướng có trọng số gồm <code>n</code> đỉnh được đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Đồ thị được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code>, trong đó mỗi cạnh <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>​​​​​​​i</sub>]</code> cho biết có một cạnh vô hướng giữa các đỉnh <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> với trọng số <code>w<sub>​​​​​​​i</sub></code>.</p>

<p>Bạn cũng được cho các số nguyên <code>source</code>, <code>target</code> và <code>k</code>.</p>

<p>Giá trị <code>threshold</code> quyết định một cạnh là <strong>nhẹ</strong> hay <strong>nặng</strong>:</p>

<ul>
	<li>
	<p>Một cạnh là <strong>nhẹ</strong> nếu trọng số của nó <strong>nhỏ hơn</strong> hoặc <strong>bằng</strong> <code>threshold</code>.</p>
	</li>
	<li>
	<p>Một cạnh là <strong>nặng</strong> nếu trọng số của nó <strong>lớn hơn</strong> <code>threshold</code>.</p>
	</li>
</ul>

<p>Một đường đi từ <code>source</code> đến <code>target</code> là <strong>hợp lệ</strong> nếu chứa <strong>không quá</strong> <code>k</code> cạnh nặng.</p>

<p>Hãy trả về <code>threshold</code> nguyên <strong>nhỏ nhất</strong> sao cho tồn tại <strong>ít nhất</strong> một đường đi <strong>hợp lệ</strong> từ <code>source</code> đến <code>target</code>. Nếu không tồn tại đường đi như vậy, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong>​​​​​​​​​​​​​​</p>

<p>​​​​​​​<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3924.Minimum%20Threshold%20Path%20With%20Limited%20Heavy%20Edges/images/g6.png" style="width: 324px; height: 200px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, edges = [[0,1,5],[1,2,3],[3,4,4],[4,5,1],[1,4,2]], source = 0, target = 3, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>threshold</code> nhỏ nhất để đường đi từ đỉnh 0 đến đỉnh 3 sử dụng không quá 1 cạnh nặng là 4.</p>

<ul>
	<li>
	<p>Các cạnh nhẹ: <code>[1, 2, 3]</code>, <code>[3, 4, 4]</code>, <code>[4, 5, 1]</code>, <code>[1, 4, 2]</code></p>
	</li>
	<li>
	<p>Cạnh nặng: <code>[0, 1, 5]</code></p>
	</li>
</ul>

<p>Một đường đi hợp lệ là <code>0 &rarr; 1 &rarr; 4 &rarr; 3</code>. Đường đi này chỉ sử dụng 1 cạnh nặng (<code>[0, 1, 5]</code>), thỏa mãn giới hạn <code>k = 1</code>.</p>

<p>Nếu <code>threshold</code> nhỏ hơn, ta không thể đến đỉnh 3 mà không vượt quá 1 cạnh nặng.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3924.Minimum%20Threshold%20Path%20With%20Limited%20Heavy%20Edges/images/g3_f.png" style="width: 324px; height: 162px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, edges = [[0,1,3],[1,2,4],[3,4,5],[4,5,6]], source = 0, target = 4, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có đường đi từ đỉnh 0 đến đỉnh 4. Vì không thể đến đỉnh đích nên kết quả là -1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3924.Minimum%20Threshold%20Path%20With%20Limited%20Heavy%20Edges/images/g5.png" style="width: 309px; height: 203px;" /></strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1,2],[1,2,2],[2,3,2],[3,0,2]], source = 0, target = 0, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đỉnh nguồn và đỉnh đích là cùng một đỉnh. Không cần đi qua cạnh nào, nên <code>threshold</code> nhỏ nhất là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>3</sup>​​​​​​​</code></li>
	<li><code>0 &lt;= edges.length &lt;= 10<sup>3</sup>​​​​​​​</code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub>​​​​​​​ &lt;= n - 1</code></li>
	<li><code>1 &lt;= w<sub>i</sub>​​​​​​​ &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= source, target &lt;= n - 1</code></li>
	<li><code>0 &lt;= k &lt;= edges.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n,m\le 10^3$, thử từng trọng số cạnh làm threshold rồi chạy đường đi ngắn nhất vẫn nằm trong giới hạn, nhưng bài toán quyết định sẽ rõ ràng hơn. Với một threshold, mỗi cạnh là nhẹ hoặc nặng, và một đường đi hợp lệ sử dụng không quá $k$ cạnh nặng.
>
> Cố định $T$, coi $w\le T$ có chi phí $0$ và $w>T$ có chi phí $1$: tồn tại đường đi hợp lệ khi và chỉ khi đường đi ngắn nhất $0$– $1$ có chi phí không quá $k$. Mệnh đề này đơn điệu theo $T$, nên ta có thể binary search trên các trọng số cạnh.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng ở “binary search kết hợp với đường đi ngắn nhất theo số cạnh nặng”.

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
