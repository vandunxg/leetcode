---
comments: true
difficulty: Medium
rating: 1883
source: Biweekly Contest 182 Q3
tags:
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [3923. Minimum Generations to Target Point](https://leetcode.com/problems/minimum-generations-to-target-point)

[中文文档](/solution/3900-3999/3923.Minimum%20Generations%20to%20Target%20Point/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>, z<sub>i</sub>]</code> biểu diễn một điểm trong không gian 3D, cùng một mảng số nguyên <code>target</code> biểu diễn điểm đích.</p>

<p>Định nghĩa <strong>thế hệ</strong> 0 là danh sách điểm ban đầu. Với mỗi số nguyên <code>k &gt;= 1</code>, thế hệ <code>k</code> được tạo như sau:</p>

<ul>
	<li>Xét mọi cặp gồm hai điểm <strong>khác nhau</strong> <code>a = [x<sub>1</sub>, y<sub>1</sub>, z<sub>1</sub>]</code> và <code>b = [x<sub>2</sub>, y<sub>2</sub>, z<sub>2</sub>]</code> lấy từ tất cả các điểm được tạo trong các thế hệ từ 0 đến <code>k - 1</code>.</li>
	<li>Với mỗi cặp như vậy, tính <code>c = [floor((x<sub>1</sub> + x<sub>2</sub>) / 2), floor((y<sub>1</sub> + y<sub>2</sub>) / 2), floor((z<sub>1</sub> + z<sub>2</sub>) / 2)]</code> rồi tập hợp tất cả các <code>c</code> như vậy vào thế hệ <code>k</code>.</li>
	<li>Tất cả các điểm trong thế hệ <code>k</code> được tạo <strong>đồng thời</strong> từ các điểm thuộc các thế hệ từ 0 đến​​​​​​​ <code>k - 1</code>.</li>
	<li>Sau khi thế hệ <code>k</code> được tạo, các điểm trong thế hệ <code>k</code> được xem là có thể sử dụng để tạo các thế hệ sau.</li>
</ul>

<p>Hãy trả về số nguyên <strong>nhỏ nhất</strong> <code>k</code> sao cho <code>target</code> xuất hiện trong một thế hệ từ 0 đến <code>k</code>. Nếu <code>target</code> đã có trong các điểm ban đầu, hãy trả về 0. Nếu không thể tạo ra <code>target</code>, hãy trả về -1.</p>

<p>Lưu ý:</p>

<ul>
	<li><strong>floor</strong> biểu thị phép làm tròn <strong>xuống</strong> đến số nguyên gần nhất.</li>
	<li>"Hai điểm <strong>khác nhau</strong>" nghĩa là hai điểm được chọn phải có tọa độ <code>(x, y, z)</code> <strong>khác nhau</strong>. Không thể ghép một điểm với chính nó, cũng như không thể ghép hai điểm có tọa độ <strong>giống hệt nhau</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[0,0,0],[6,6,6]], target = [3,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Thế hệ 0:</strong> Các điểm ban đầu là <code>points = [[0, 0, 0], [6, 6, 6]]</code>.</li>
	<li><code>target = [3, 3, 3]</code> không tồn tại trong thế hệ 0.</li>
	<li><strong>Thế hệ 1:</strong> Với mỗi cặp điểm trong thế hệ 0, ta tạo các điểm mới.
	<ul>
		<li>Dùng <code>[0, 0, 0]</code> và <code>[6, 6, 6]</code>, ta tạo ra <code>[3, 3, 3]</code>.</li>
	</ul>
	</li>
	<li>Sau thế hệ 1, <code>points = [[0, 0, 0], [6, 6, 6], [3, 3, 3]]</code>.</li>
	<li>Tìm thấy <code>target = [3, 3, 3]</code> trong thế hệ 1, nên <code>k</code> nhỏ nhất là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[0,0,0],[5,5,5]], target = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Thế hệ 0:</strong> Các điểm ban đầu là <code>points = [[0, 0, 0], [5, 5, 5]]</code>.</li>
	<li><code>target = [1, 1, 1]</code> không tồn tại trong thế hệ 0.</li>
	<li><strong>Thế hệ 1:</strong> Với mỗi cặp điểm trong thế hệ 0, ta tạo các điểm mới.
	<ul>
		<li>Dùng <code>[0, 0, 0]</code> và <code>[5, 5, 5]</code>, ta tạo ra <code>[2, 2, 2]</code>.</li>
	</ul>
	</li>
	<li>Sau thế hệ 1, <code>points = [[0, 0, 0], [5, 5, 5], [2, 2, 2]]</code>.</li>
	<li><strong>Thế hệ 2:</strong> Với mỗi cặp điểm có thể sử dụng sau thế hệ 1, ta tạo các điểm mới.
	<ul>
		<li>Dùng <code>[0, 0, 0]</code> và <code>[5, 5, 5]</code>, ta tạo ra <code>[2, 2, 2]</code>.</li>
		<li>Dùng <code>[0, 0, 0]</code> và <code>[2, 2, 2]</code>, ta tạo ra <code>[1, 1, 1]</code>.</li>
		<li>Dùng <code>[5, 5, 5]</code> và <code>[2, 2, 2]</code>, ta tạo ra <code>[3, 3, 3]</code>.</li>
	</ul>
	</li>
	<li>Sau thế hệ 2, <code>points = [[0, 0, 0], [5, 5, 5], [2, 2, 2], [1, 1, 1], [3, 3, 3]]</code>.</li>
	<li>Tìm thấy <code>target = [1, 1, 1]</code> trong thế hệ 2, nên <code>k</code> nhỏ nhất là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[0,0,0],[2,2,2],[3,3,3]], target = [2,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Thế hệ 0:</strong> Các điểm ban đầu là <code>points = [[0, 0, 0], [2, 2, 2], [3, 3, 3]]</code>.</li>
	<li><code>target = [2, 2, 2]</code> đã tồn tại trong thế hệ 0, nên <code>k</code> nhỏ nhất là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,2,3]], target = [5,5,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chỉ có một điểm ban đầu, nên không thể tạo thêm điểm mới.</li>
	<li>Vì vậy, không thể tạo ra target và đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 20</code></li>
	<li><code>points[i] = [x<sub>i</sub>, y<sub>i</sub>, z<sub>i</sub>​​​​​​​]</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub>, z<sub>i</sub> &lt;= 6</code></li>
	<li><code>target.length == 3</code></li>
	<li><code>​​​​​​​0 &lt;= target[i] &lt;= 6</code></li>
	<li>Tập điểm ban đầu không chứa phần tử trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có tối đa $20$ điểm và các tọa độ không vượt quá $6$, nhưng việc mở rộng các thế hệ bằng cách ghép các trung điểm sẽ lặp lại nhiều điểm. Phép ánh xạ $\lfloor(a+b)/2\rfloor$ hoạt động độc lập trên từng tọa độ.
>
> Trong một chiều, chỉ có thể tạo ra $x$ nếu nó nằm trong bao lồi của các tọa độ hiện có và vượt qua trở ngại đồng dư do phép chia nguyên tạo ra (các bit thấp bị loại bỏ). Đáp án là giá trị $k$ nhỏ nhất sao cho điều kiện này đúng đồng thời trên cả ba chiều.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng ở phân tích theo từng trục đó.

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
