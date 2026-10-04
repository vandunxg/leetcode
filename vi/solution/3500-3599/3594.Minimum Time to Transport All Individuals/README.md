---
comments: true
difficulty: Hard
rating: 2604
source: Weekly Contest 455 Q4
tags:
    - Bit Manipulation
    - Graph
    - Array
    - Bitmask
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3594. Minimum Time to Transport All Individuals](https://leetcode.com/problems/minimum-time-to-transport-all-individuals)

[中文文档](/solution/3500-3599/3594.Minimum%20Time%20to%20Transport%20All%20Individuals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp <code>n</code> người tại một trại căn cứ, cần vượt sông để đến đích bằng một chiếc thuyền duy nhất. Thuyền có thể chở tối đa <code>k</code> người mỗi lần. Chuyến đi chịu ảnh hưởng bởi các điều kiện môi trường thay đổi <strong>theo chu kỳ</strong> qua <code>m</code> giai đoạn.</p>

<p>Mỗi giai đoạn <code>j</code> có một hệ số tốc độ <code>mul[j]</code>:</p>

<ul>
	<li>Nếu <code>mul[j] &gt; 1</code>, chuyến đi sẽ chậm hơn.</li>
	<li>Nếu <code>mul[j] &lt; 1</code>, chuyến đi sẽ nhanh hơn.</li>
</ul>

<p>Mỗi người <code>i</code> có sức chèo được biểu diễn bởi <code>time[i]</code>, là thời gian (tính bằng phút) để người đó tự mình vượt sông trong điều kiện bình thường.</p>

<p><strong>Quy tắc:</strong></p>

<ul>
	<li>Một nhóm <code>g</code> khởi hành ở giai đoạn <code>j</code> sẽ mất thời gian bằng <strong>giá trị lớn nhất</strong> của <code>time[i]</code> trong nhóm, nhân với <code>mul[j]</code> phút để đến đích.</li>
	<li>Sau khi nhóm vượt sông trong thời gian <code>d</code>, giai đoạn tăng thêm <code>floor(d) % m</code> bước.</li>
	<li>Nếu vẫn còn người ở lại, một người phải quay về cùng thuyền. Gọi <code>r</code> là chỉ số của người quay về, thời gian quay về là <code>time[r] &times; mul[current_stage]</code>, được định nghĩa là <code>return_time</code>, và giai đoạn tăng thêm <code>floor(return_time) % m</code> bước.</li>
</ul>

<p>Trả về <strong>tổng thời gian nhỏ nhất</strong> cần thiết để đưa tất cả mọi người đến đích. Nếu không thể đưa tất cả mọi người đến đích, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, k = 1, m = 2, time = [5], mul = [1.0,1.3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5.00000</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Người 0 khởi hành ở giai đoạn 0, nên thời gian vượt sông = <code>5 &times; 1.00 = 5.00</code> phút.</li>
	<li>Tất cả thành viên trong nhóm hiện đã ở đích. Do đó, tổng thời gian cần thiết là <code>5.00</code> phút.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 2, m = 3, time = [2,5,8], mul = [1.0,1.5,0.75]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14.50000</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chiến lược tối ưu là:</p>

<ul>
	<li>Đưa người 0 và 2 từ trại căn cứ đến đích ở giai đoạn 0. Thời gian vượt sông là <code>max(2, 8) &times; mul[0] = 8 &times; 1.00 = 8.00</code> phút. Giai đoạn tăng thêm <code>floor(8.00) % 3 = 2</code>, nên giai đoạn tiếp theo là <code>(0 + 2) % 3 = 2</code>.</li>
	<li>Người 0 một mình quay về từ đích đến trại căn cứ ở giai đoạn 2. Thời gian quay về là <code>2 &times; mul[2] = 2 &times; 0.75 = 1.50</code> phút. Giai đoạn tăng thêm <code>floor(1.50) % 3 = 1</code>, nên giai đoạn tiếp theo là <code>(2 + 1) % 3 = 0</code>.</li>
	<li>Đưa người 0 và 1 từ trại căn cứ đến đích ở giai đoạn 0. Thời gian vượt sông là <code>max(2, 5) &times; mul[0] = 5 &times; 1.00 = 5.00</code> phút. Giai đoạn tăng thêm <code>floor(5.00) % 3 = 2</code>, nên giai đoạn cuối là <code>(0 + 2) % 3 = 2</code>.</li>
	<li>Tất cả thành viên trong nhóm hiện đã ở đích. Tổng thời gian cần thiết là <code>8.00 + 1.50 + 5.00 = 14.50</code> phút.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, k = 1, m = 2, time = [10,10], mul = [2.0,2.0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1.00000</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì thuyền chỉ có thể chở một người mỗi lần, không thể đưa cả hai người sang sông khi luôn phải có một người quay về. Do đó, đáp án là <code>-1.00</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == time.length &lt;= 12</code></li>
	<li><code>1 &lt;= k &lt;= 5</code></li>
	<li><code>1 &lt;= m &lt;= 5</code></li>
	<li><code>1 &lt;= time[i] &lt;= 100</code></li>
	<li><code>m == mul.length</code></li>
	<li><code>0.5 &lt;= mul[i] &lt;= 2.0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 12$, $k \le 5$, $m \le 5$, một trạng thái gồm (bit mask của những người vẫn còn ở trại, phía mà thuyền đang ở, giai đoạn hiện tại). Thời gian vượt sông là giá trị lớn nhất của $\textit{time}$ trên thuyền nhân với hệ số của giai đoạn; giai đoạn tăng thêm $\lfloor d \rfloor$.
>
> Dijkstra tính thời gian sớm nhất trên đồ thị đó. Khi thuyền ở phía bên kia, một người phải quay về. Nếu mask của trại không bao giờ trở thành rỗng, trả về $-1$.

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
