---
comments: true
difficulty: Hard
rating: 2843
source: Biweekly Contest 147 Q4
tags:
    - Segment Tree
    - Array
    - Hash Table
    - Divide and Conquer
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3410. Maximize Subarray Sum After Removing All Occurrences of One Element](https://leetcode.com/problems/maximize-subarray-sum-after-removing-all-occurrences-of-one-element)

[中文文档](/solution/3400-3499/3410.Maximize%20Subarray%20Sum%20After%20Removing%20All%20Occurrences%20of%20One%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>nhiều nhất</strong> một lần:</p>

<ul>
	<li>Chọn <strong>bất kỳ</strong> số nguyên <code>x</code> nào sao cho <code>nums</code> vẫn <strong>không rỗng</strong> sau khi xóa mọi lần xuất hiện của <code>x</code>.</li>
	<li>Xóa&nbsp;<strong>mọi</strong> lần xuất hiện của <code>x</code> khỏi mảng.</li>
</ul>

<p>Trả về <strong>tổng lớn nhất</strong> của <span data-keyword="subarray-nonempty">mảng con</span> trên <strong>tất cả</strong> các mảng có thể nhận được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-3,2,-2,-1,3,-2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể nhận được các mảng sau khi thực hiện nhiều nhất một thao tác:</p>

<ul>
	<li>Mảng ban đầu là <code>nums = [<span class="example-io">-3, 2, -2, -1, <u><strong>3, -2, 3</strong></u></span>]</code>. Tổng lớn nhất của mảng con là <code>3 + (-2) + 3 = 4</code>.</li>
	<li>Xóa mọi lần xuất hiện của <code>x = -3</code> cho kết quả <code>nums = [2, -2, -1, <strong><u><span class="example-io">3, -2, 3</span></u></strong>]</code>. Tổng lớn nhất của mảng con là <code>3 + (-2) + 3 = 4</code>.</li>
	<li>Xóa mọi lần xuất hiện của <code>x = -2</code> cho kết quả <code>nums = [<span class="example-io">-3, <strong><u>2, -1, 3, 3</u></strong></span>]</code>. Tổng lớn nhất của mảng con là <code>2 + (-1) + 3 + 3 = 7</code>.</li>
	<li>Xóa mọi lần xuất hiện của <code>x = -1</code> cho kết quả <code>nums = [<span class="example-io">-3, 2, -2, <strong><u>3, -2, 3</u></strong></span>]</code>. Tổng lớn nhất của mảng con là <code>3 + (-2) + 3 = 4</code>.</li>
	<li>Xóa mọi lần xuất hiện của <code>x = 3</code> cho kết quả <code>nums = [<span class="example-io">-3, <u><strong>2</strong></u>, -2, -1, -2</span>]</code>. Tổng lớn nhất của mảng con là 2.</li>
</ul>

<p>Kết quả là <code>max(4, 4, 7, 4, 2) = 7</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thực hiện thao tác nào là phương án tối ưu.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>6</sup> &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tổng lớn nhất của mảng con thông thường có thể được tính trong thời gian tuyến tính bằng Kadane. Ở đây, ta có thể xóa mọi lần xuất hiện của một giá trị, tức là coi các vị trí chứa giá trị đó đã bị loại bỏ rồi tìm mảng con lớn nhất.
>
> Với $n\le 10^5$, ta không thể xây dựng lại mảng cho từng giá trị phân biệt. Việc xóa $x$ phải được biểu diễn dưới dạng thay đổi các đóng góp của mảng ban đầu.
>
> Xóa một $x$ không âm không thể mang lại lợi ích. Xóa một $x$ âm sẽ loại bỏ nhiều đóng góp âm. Ta nhóm các chỉ số theo giá trị và gộp các đoạn Kadane mà $x$ từng chia tách, sau đó lấy kết quả tốt nhất trên mọi lựa chọn $x$ (bao gồm cả không xóa gì).

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
