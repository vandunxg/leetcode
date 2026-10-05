---
comments: true
difficulty: Hard
rating: 2263
source: Weekly Contest 500 Q4
---

<!-- problem:start -->

# [3920. Maximize Fixed Points After Deletions](https://leetcode.com/problems/maximize-fixed-points-after-deletions)

[中文文档](/solution/3900-3999/3920.Maximize%20Fixed%20Points%20After%20Deletions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một vị trí <code>i</code> được gọi là <strong>điểm cố định</strong> nếu <code>nums[i] == i</code>.</p>

<p>Bạn được phép xóa <strong>bất kỳ</strong> số lượng phần tử nào (kể cả không xóa phần tử nào) khỏi mảng. Sau mỗi lần xóa, các phần tử còn lại <strong>dịch sang trái</strong> và các chỉ số được gán lại bắt đầu từ 0.</p>

<p>Trả về một số nguyên biểu thị số lượng điểm cố định <strong>lớn nhất</strong> có thể đạt được sau khi thực hiện bất kỳ số lần xóa nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa <code>nums[1] = 2</code>. Mảng trở thành <code>[0, 1]</code>.</li>
	<li>Bây giờ, <code>nums[0] = 0</code> và <code>nums[1] = 1</code>, nên cả hai chỉ số đều là điểm cố định.</li>
	<li>Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không xóa phần tử nào. Mảng vẫn là <code>[3, 1, 2]</code>.</li>
	<li>Ở đây, <code>nums[1] = 1</code> và <code>nums[2] = 2</code>, nên các chỉ số này là điểm cố định.</li>
	<li>Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa <code>nums[0] = 1</code>. Mảng trở thành <code>[0, 1, 2]</code>.</li>
	<li>Bây giờ, <code>nums[0] = 0</code>, <code>nums[1] = 1</code> và <code>nums[2] = 2</code>, nên tất cả các chỉ số đều là điểm cố định.</li>
	<li>Do đó, đáp án là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các phép xóa làm thay đổi chỉ số của những phần tử phía sau, nên việc liệt kê các tập hợp phần tử cần xóa có số lượng trường hợp theo cấp số mũ và không thể thực hiện khi $n\le 10^5$. Một giá trị $x$ trở thành điểm cố định chỉ khi nó được đặt tại chỉ số $x$, nghĩa là còn lại chính xác $x$ phần tử ở bên trái nó.
>
> Do đó, mỗi $x$ đóng góp nhiều nhất một điểm cố định, và một ứng viên phải thỏa mãn $\textit{nums}[i]$ đủ lớn so với chỉ số cuối cùng của nó. Bài toán là giữ lại nhiều vị trí như vậy nhất có thể mà không phá vỡ độ dài prefix cần thiết bởi các điểm cố định nhỏ hơn.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng lại ở mối tương ứng giữa chỉ số và giá trị.

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
