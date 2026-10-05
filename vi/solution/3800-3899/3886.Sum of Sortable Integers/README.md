---
comments: true
difficulty: Hard
rating: 1999
source: Weekly Contest 495 Q3
tags:
    - Array
    - Math
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3886. Sum of Sortable Integers](https://leetcode.com/problems/sum-of-sortable-integers)

[中文文档](/solution/3800-3899/3886.Sum%20of%20Sortable%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Một số nguyên <code>k</code> được gọi là <strong>có thể sắp xếp</strong> nếu <code>k</code> <strong>là ước</strong> của <code>n</code> và bạn có thể sắp xếp <code>nums</code> theo <strong>thứ tự không giảm</strong> bằng cách lần lượt thực hiện các thao tác sau:</p>

<ul>
	<li>Chia <code>nums</code> thành các <strong><span data-keyword="subarray-nonempty">mảng con</span> liên tiếp</strong> có độ dài <code>k</code>.</li>
	<li><strong>Xoay vòng độc lập từng mảng con</strong> sang trái hoặc sang phải số lần bất kỳ.</li>
</ul>

<p>Trả về một số nguyên biểu thị tổng của tất cả các số nguyên <code>k</code> có thể sắp xếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Với <code>n = 3</code>, các ước có thể là 1 và 3.</li>
	<li>Với <code>k = 1</code>: mỗi mảng con có một phần tử. Không phép xoay nào có thể sắp xếp mảng.</li>
	<li>Với <code>k = 3</code>: mảng con duy nhất <code>[3, 1, 2]</code> có thể được xoay một lần để tạo thành <code>[1, 2, 3]</code>, là một mảng đã được sắp xếp.</li>
	<li>Chỉ có <code>k = 3</code> là có thể sắp xếp. Do đó, đáp án là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,6,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>n = 3</code>, các ước có thể là 1 và 3.</li>
	<li>Với <code>k = 1</code>: mỗi mảng con có một phần tử. Không phép xoay nào có thể sắp xếp mảng.</li>
	<li>Với <code>k = 3</code>: mảng con duy nhất <code>[7, 6, 5]</code> không thể được xoay để có thứ tự không giảm.</li>
	<li>Không có <code>k</code> nào có thể sắp xếp. Do đó, đáp án là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Với <code>n = 2</code>, các ước có thể là 1 và 2.</li>
	<li>Vì <code>[5, 8]</code> đã được sắp xếp, mọi ước đều có thể sắp xếp. Do đó, đáp án là <code>1 + 2 = 3</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $k$ là số có thể sắp xếp khi và chỉ khi $k$ là ước của $n$ và mỗi khối có độ dài $k$ có thể được xoay sao cho phép nối các khối có thứ tự không giảm. $n \le 10^5$.
>
> Có ít ước, nên ta liệt kê chúng. Với mỗi $k$, mỗi khối phải là một phép xoay, và các khối liền kề phải nối với nhau theo thứ tự không giảm.
>
> Tính đơn điệu trên toàn mảng có nghĩa là các phép xoay được chọn phải nối tiếp nhau theo đúng thứ tự. Ta kiểm tra từng vị trí dựa trên thứ tự vòng, hoặc so sánh các khối liền kề sau khi xoay chúng ít nhất có thể.
>
> Cộng các ước vượt qua phép kiểm tra.

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
