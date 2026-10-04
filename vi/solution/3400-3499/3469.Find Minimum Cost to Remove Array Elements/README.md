---
comments: true
difficulty: Medium
rating: 2111
source: Biweekly Contest 151 Q3
---

<!-- problem:start -->

# [3469. Find Minimum Cost to Remove Array Elements](https://leetcode.com/problems/find-minimum-cost-to-remove-array-elements)

[中文文档](/solution/3400-3499/3469.Find%20Minimum%20Cost%20to%20Remove%20Array%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Nhiệm vụ của bạn là xóa <strong>tất cả phần tử</strong> khỏi mảng bằng cách thực hiện một trong các thao tác sau ở mỗi bước cho đến khi <code>nums</code> rỗng:</p>

<ul>
	<li>Chọn hai phần tử bất kỳ trong ba phần tử đầu tiên của <code>nums</code> và xóa chúng. Chi phí của thao tác này là <strong>giá trị lớn nhất</strong> trong hai phần tử bị xóa.</li>
	<li>Nếu <code>nums</code> còn ít hơn ba phần tử, xóa tất cả phần tử còn lại trong một thao tác. Chi phí của thao tác này là <strong>giá trị lớn nhất</strong> trong các phần tử còn lại.</li>
</ul>

<p>Trả về chi phí <strong>nhỏ nhất</strong> cần thiết để xóa tất cả phần tử.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,2,8,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, <code>nums = [6, 2, 8, 4]</code>.</p>

<ul>
	<li>Trong thao tác đầu tiên, xóa <code>nums[0] = 6</code> và <code>nums[2] = 8</code> với chi phí <code>max(6, 8) = 8</code>. Khi đó, <code>nums = [2, 4]</code>.</li>
	<li>Trong thao tác thứ hai, xóa các phần tử còn lại với chi phí <code>max(2, 4) = 4</code>.</li>
</ul>

<p>Chi phí để xóa tất cả phần tử là <code>8 + 4 = 12</code>. Đây là chi phí nhỏ nhất để xóa tất cả phần tử trong <code>nums</code>. Do đó, đầu ra là 12.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, <code>nums = [2, 1, 3, 3]</code>.</p>

<ul>
	<li>Trong thao tác đầu tiên, xóa <code>nums[0] = 2</code> và <code>nums[1] = 1</code> với chi phí <code>max(2, 1) = 2</code>. Khi đó, <code>nums = [3, 3]</code>.</li>
	<li>Trong thao tác thứ hai, xóa các phần tử còn lại với chi phí <code>max(3, 3) = 3</code>.</li>
</ul>

<p>Chi phí để xóa tất cả phần tử là <code>2 + 3 = 5</code>. Đây là chi phí nhỏ nhất để xóa tất cả phần tử trong <code>nums</code>. Do đó, đầu ra là 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác loại bỏ ba phần tử (theo đề bài) với chi phí bằng giá trị lớn nhất của chúng, cho đến khi mảng được xóa hết. Vì $n\le 1000$, việc xét thứ tự xóa sẽ có số lượng trường hợp tăng theo cấp số mũ.
>
> Phần còn lại gồm một prefix vẫn đang được xử lý và nhiều nhất một giá trị được giữ lại, tạo thành một trạng thái gọn.
>
> DP có memoization liệt kê các chỉ số sẽ được xóa tiếp theo và cộng giá trị lớn nhất của chúng. Một hoặc hai phần tử cuối là các trường hợp cơ sở.

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
