---
comments: true
difficulty: Hard
rating: 2345
source: Biweekly Contest 175 Q4
tags:
    - Queue
    - Array
    - Divide and Conquer
    - Dynamic Programming
    - Prefix Sum
    - Monotonic Queue
---

<!-- problem:start -->

# [3826. Minimum Partition Score](https://leetcode.com/problems/minimum-partition-score)

[中文文档](/solution/3800-3899/3826.Minimum%20Partition%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là phân hoạch <code>nums</code> thành <strong>đúng</strong> <code>k</code> <span data-keyword="subarray-nonempty">mảng con</span> và trả về một số nguyên biểu thị <strong>điểm nhỏ nhất có thể</strong> trong tất cả các phân hoạch hợp lệ.</p>

<p><strong>Điểm</strong> của một phân hoạch là <strong>tổng</strong> <strong>giá trị</strong> của tất cả các mảng con.</p>

<p><strong>Giá trị</strong> của một mảng con được định nghĩa là <code>sumArr * (sumArr + 1) / 2</code>, trong đó <code>sumArr</code> là tổng các phần tử của mảng con đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,1,2,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta phải phân hoạch mảng thành <code>k = 2</code> mảng con. Một phân hoạch tối ưu là <code>[5]</code> và <code>[1, 2, 1]</code>.</li>
	<li>Mảng con đầu tiên có <code>sumArr = 5</code> và <code>value = 5 &times; 6 / 2 = 15</code>.</li>
	<li>Mảng con thứ hai có <code>sumArr = 1 + 2 + 1 = 4</code> và <code>value = 4 &times; 5 / 2 = 10</code>.</li>
	<li>Điểm của phân hoạch này là <code>15 + 10 = 25</code>, đây là điểm nhỏ nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">55</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì ta phải phân hoạch mảng thành <code>k = 1</code> mảng con, tất cả phần tử thuộc cùng một mảng con: <code>[1, 2, 3, 4]</code>.</li>
	<li>Mảng con này có <code>sumArr = 1 + 2 + 3 + 4 = 10</code> và <code>value = 10 &times; 11 / 2 = 55</code>.​​​​​​​</li>
	<li>Điểm của phân hoạch này là 55, đây là điểm nhỏ nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta phải phân hoạch mảng thành <code>k = 3</code> mảng con. Phân hoạch hợp lệ duy nhất là <code>[1], [1], [1]</code>.</li>
	<li>Mỗi mảng con có <code>sumArr = 1</code> và <code>value = 1 &times; 2 / 2 = 1</code>.</li>
	<li>Điểm của phân hoạch này là <code>1 + 1 + 1 = 3</code>, đây là điểm nhỏ nhất có thể.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length </code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chia mảng thành đúng $k$ phần, mỗi phần có điểm $\textit{sum}(\textit{sum}+1)/2$, rồi tối thiểu hóa tổng điểm. Với $n \le 1000$, độ phức tạp $O(n^2 k)$ ở mức sát giới hạn; các hằng số cũng rất quan trọng.
>
> Bài toán phân hoạch có cấu trúc tối ưu: cách chia một tiền tố thành $t$ phần tốt nhất chỉ phụ thuộc vào các cách chia những tiền tố ngắn hơn thành $(t-1)$ phần.
>
> Chi phí của mỗi phần là hàm lồi theo tổng tiền tố, nên các quyết định liền kề có tính đơn điệu và một deque có thể giảm mỗi lớp từ bậc hai xuống tuyến tính.
>
> Vì vậy, ta dùng DP theo số phần, tính chi phí của một phần từ các tổng tiền tố, đồng thời duy trì hàng đợi các quyết định do tính lồi này suy ra.

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
