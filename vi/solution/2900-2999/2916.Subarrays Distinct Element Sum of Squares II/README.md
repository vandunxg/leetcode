---
comments: true
difficulty: Hard
rating: 2816
source: Biweekly Contest 116 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2916. Subarrays Distinct Element Sum of Squares II](https://leetcode.com/problems/subarrays-distinct-element-sum-of-squares-ii)

[中文文档](/solution/2900-2999/2916.Subarrays%20Distinct%20Element%20Sum%20of%20Squares%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0 </strong><code>nums</code>.</p>

<p><strong>Số lượng phần tử phân biệt</strong> của một mảng con của <code>nums</code> được định nghĩa như sau:</p>

<ul>
	<li>Gọi <code>nums[i..j]</code> là một mảng con của <code>nums</code> gồm tất cả các chỉ số từ <code>i</code> đến <code>j</code> sao cho <code>0 &lt;= i &lt;= j &lt; nums.length</code>. Khi đó, số lượng giá trị phân biệt trong <code>nums[i..j]</code> được gọi là số lượng phần tử phân biệt của <code>nums[i..j]</code>.</li>
</ul>

<p>Trả về <em>tổng <strong>bình phương</strong> của <strong>số lượng phần tử phân biệt</strong> của tất cả các mảng con của </em><code>nums</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Mảng con là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Có sáu mảng con:
[1]: 1 giá trị phân biệt
[2]: 1 giá trị phân biệt
[1]: 1 giá trị phân biệt
[1,2]: 2 giá trị phân biệt
[2,1]: 2 giá trị phân biệt
[1,2,1]: 2 giá trị phân biệt
Tổng bình phương của số lượng phần tử phân biệt trong tất cả các mảng con là 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> = 15.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có ba mảng con:
[2]: 1 giá trị phân biệt
[2]: 1 giá trị phân biệt
[2,2]: 1 giá trị phân biệt
Tổng bình phương của số lượng phần tử phân biệt trong tất cả các mảng con là 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> = 3.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tổng này giống như ở phần I, nhưng $n \le 10^5$ khiến việc liệt kê các mảng con là không thể. Khi đầu phải di chuyển từ $r-1$ sang $r$, các mảng con mới đều có dạng $[L,r]$. Nếu $x=nums[r]$ xuất hiện gần nhất trước đó tại $p$, mọi $L \in (p,r]$ đều tăng thêm một giá trị phân biệt, nên tổng bình phương tăng thêm $2 \cdot \mathrm{cnt}+1$.
>
> Đây là phép cộng trên một đoạn chỉ số kết hợp với truy vấn tổng bình phương, có thể được lưu bằng segment tree lazy. Ta duyệt theo đầu phải, cộng một trên $(last[x], r]$, rồi cộng dồn tổng bình phương trên toàn bộ mảng. Các tab code trong thư mục này vẫn còn trống; cấu trúc trên chính là trạng thái và thứ tự cập nhật dự kiến.

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
