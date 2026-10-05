---
comments: true
difficulty: Hard
rating: 2279
source: Biweekly Contest 189 Q4
tags:
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [4023. Elevator Requests II](https://leetcode.com/problems/elevator-requests-ii)

[中文文档](/solution/4000-4099/4023.Elevator%20Requests%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số tầng trong một tòa nhà, trong đó các tầng được đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Bạn cũng được cho một số nguyên <code>start</code>, biểu thị tầng nơi thang máy bắt đầu, và một mảng số nguyên <code>requests</code>, trong đó <code>requests[i]</code> là tầng mà thang máy được yêu cầu đi tới. Tất cả các tầng trong <code>requests</code> đều <strong>khác nhau</strong>.</p>

<p>Tại thời điểm 0, thang máy đang ở tầng <code>start</code>, và tất cả các yêu cầu được đưa ra <strong>đồng thời</strong>.</p>

<p>Trong mỗi giây trước khi hoàn thành tất cả yêu cầu, thang máy di chuyển <strong>chính xác</strong> một tầng, theo hướng <strong>lên</strong> hoặc <strong>xuống</strong>. Một yêu cầu được hoàn thành <strong>ngay lập tức</strong> khi thang máy đến tầng được yêu cầu. Nếu <code>start</code> xuất hiện trong <code>requests</code>, yêu cầu đó được hoàn thành tại thời điểm 0.</p>

<p>Với mỗi giây một yêu cầu chưa được hoàn thành, bạn nhận 1 điểm phạt. Tương đương, một yêu cầu được hoàn thành tại thời điểm <code>t</code> sẽ đóng góp <code>t</code> vào tổng điểm phạt.</p>

<p>Trả về tổng điểm phạt <strong>nhỏ nhất</strong> cần thiết để hoàn thành tất cả yêu cầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, start = 4, requests = [1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Di chuyển từ tầng 4 (<code>start</code>) đến tầng 5 trong 1 giây. Điểm phạt cho tầng 5 là 1.</li>
	<li>Di chuyển từ tầng 5 đến tầng 1 trong 4 giây. Điểm phạt cho tầng 1 là 5.</li>
</ul>

<p>Vậy tổng điểm phạt là <code>1 + 5 = 6</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 8, start = 3, requests = [3,7,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tầng 3 (<code>start</code>) được hoàn thành ngay lập tức. Điểm phạt cho tầng 3 là 0.</li>
	<li>Di chuyển từ tầng 3 đến tầng 1 trong 2 giây. Điểm phạt cho tầng 1 là 2.</li>
	<li>Di chuyển từ tầng 1 đến tầng 7 trong 6 giây. Điểm phạt cho tầng 7 là 8.</li>
</ul>

<p>Vậy tổng điểm phạt là <code>0 + 2 + 8 = 10</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10, start = 5, requests = [0,2,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Di chuyển từ tầng 5 (<code>start</code>) đến tầng 2 trong 3 giây. Điểm phạt cho tầng 2 là 3.</li>
	<li>Di chuyển từ tầng 2 đến tầng 0 trong 2 giây. Điểm phạt cho tầng 0 là 5.</li>
	<li>Di chuyển từ tầng 0 đến tầng 9 trong 9 giây. Điểm phạt cho tầng 9 là 14.</li>
</ul>

<p>Vậy tổng điểm phạt là <code>3 + 5 + 14 = 22</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= requests.length &lt;= 1500</code></li>
	<li><code>0 &lt;= start, requests[i] &lt;= n - 1</code></li>
	<li>Tất cả các giá trị trong <code>requests</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số tầng có thể lên tới $10^9$, nhưng chỉ có $m\le 1500$ yêu cầu khác nhau, nên lộ trình chỉ đi qua các tầng đó. Thang máy di chuyển một tầng mỗi giây và điểm phạt là tổng thời điểm ghé đến, do đó thứ tự ghé đến quyết định kết quả.
>
> $m$ đã quá lớn để dùng subset TSP, nhưng các điểm nằm trên một đường thẳng, nên lộ trình tối ưu là một chuỗi các lượt quét một chiều.
>
> Sau khi sắp xếp các yêu cầu theo tầng, interval DP $O(m^2)$ sẽ liệt kê đoạn quét tiếp theo mà không cần xây dựng đồ thị trên $10^9$ tầng.

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
