---
comments: true
difficulty: Hard
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3929. Minimum Partition Score II 🔒](https://leetcode.com/problems/minimum-partition-score-ii)

[中文文档](/solution/3900-3999/3929.Minimum%20Partition%20Score%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là chia <code>nums</code> thành <strong>chính xác</strong> <code>k</code> <span data-keyword="subarray-nonempty">mảng con</span> và trả về một số nguyên biểu thị <strong>điểm nhỏ nhất có thể</strong> trong tất cả các cách chia hợp lệ.</p>

<p><strong>Điểm</strong> của một cách chia là <strong>tổng</strong> <strong>giá trị</strong> của tất cả các mảng con.</p>

<p><strong>Giá trị</strong> của một mảng con được định nghĩa là <code>sumArr * (sumArr + 1) / 2</code>, trong đó <code>sumArr</code> là tổng các phần tử của mảng con đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,1,2,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta phải chia mảng thành <code>k = 2</code> mảng con. Một cách chia tối ưu là <code>[5]</code> và <code>[1, 2, 1]</code>.</li>
	<li>Mảng con thứ nhất có <code>sum = 5</code> và <code>value = 5 * 6 / 2 = 15</code>.</li>
	<li>Mảng con thứ hai có <code>sum = 1 + 2 + 1 = 4</code> và <code>value = 4 * 5 / 2 = 10</code>.</li>
	<li>Điểm của cách chia này là <code>15 + 10 = 25</code>, đây là điểm nhỏ nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">55</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì ta phải chia mảng thành <code>k = 1</code> mảng con, tất cả phần tử thuộc cùng một mảng con: <code>[1, 2, 3, 4]</code>.</li>
	<li>Mảng con này có <code>sum = 1 + 2 + 3 + 4 = 10</code> và <code>value = 10 * 11 / 2 = 55</code>.​​​​​​​</li>
	<li>Điểm của cách chia này là 55, đây là điểm nhỏ nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta phải chia mảng thành <code>k = 3</code> mảng con. Cách chia hợp lệ duy nhất là <code>[1], [1], [1]</code>.</li>
	<li>Mỗi mảng con có <code>sum = 1</code> và <code>value = 1 * 2 / 2 = 1</code>.</li>
	<li>Điểm của cách chia này là <code>1 + 1 + 1 = 3</code>, đây là điểm nhỏ nhất có thể.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị của một đoạn là $\mathrm{sum}(\mathrm{sum}+1)/2$, nên điểm của cách chia là hàm bậc hai theo tổng các đoạn. Quy hoạch động đơn giản trên các vị trí chia có độ phức tạp $O(n^2k)$, không đáp ứng được với $n\le 5\times 10^4$.
>
> Chi phí bậc hai gợi ý dùng quy hoạch động chia thành $k$ phần, được tăng tốc bằng convex hull hoặc bất đẳng thức tứ giác. Với tổng tiền tố $s_i$, $f[i][t]$ là chi phí nhỏ nhất của $i$ phần tử đầu tiên trong $t$ đoạn, với phép chuyển $f[j][t-1]+(s_i-s_j)(s_i-s_j+1)/2$.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng lại ở quy hoạch động chia thành $k$ phần đó.

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
