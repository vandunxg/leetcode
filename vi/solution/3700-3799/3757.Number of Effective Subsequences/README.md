---
comments: true
difficulty: Hard
rating: 2519
source: Weekly Contest 477 Q4
tags:
    - Bit Manipulation
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [3757. Number of Effective Subsequences](https://leetcode.com/problems/number-of-effective-subsequences)

[中文文档](/solution/3700-3799/3757.Number%20of%20Effective%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p><strong>Độ mạnh</strong> của mảng được định nghĩa là <strong>phép OR bitwise</strong> của tất cả các phần tử trong mảng.</p>

<p>Một <strong><span data-keyword="subsequence-array-nonempty">dãy con</span></strong> được gọi là <strong>hiệu quả</strong> nếu việc xóa dãy con đó làm <strong>giảm nghiêm ngặt</strong> độ mạnh của các phần tử còn lại.</p>

<p>Trả về số lượng <strong>dãy con hiệu quả</strong> trong <code>nums</code>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Phép OR bitwise của một mảng rỗng là 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phép OR bitwise của mảng là <code>1 OR 2 OR 3 = 3</code>.</li>
	<li>Các dãy con hiệu quả là:
	<ul>
		<li><code>[1, 3]</code>: Phần tử còn lại <code>[2]</code> có phép OR bitwise bằng 2.</li>
		<li><code>[2, 3]</code>: Phần tử còn lại <code>[1]</code> có phép OR bitwise bằng 1.</li>
		<li><code>[1, 2, 3]</code>: Các phần tử còn lại <code>[]</code> có phép OR bitwise bằng 0.</li>
	</ul>
	</li>
	<li>Vậy tổng số dãy con hiệu quả là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Phép OR bitwise của mảng là <code>7 OR 4 OR 6 = 7</code>.</li>
	<li>Các dãy con hiệu quả là:
	<ul>
		<li><code>[7]</code>: Các phần tử còn lại <code>[4, 6]</code> có phép OR bitwise bằng 6.</li>
		<li><code>[7, 4]</code>: Phần tử còn lại <code>[6]</code> có phép OR bitwise bằng 6.</li>
		<li><code>[7, 6]</code>: Phần tử còn lại <code>[4]</code> có phép OR bitwise bằng 4.</li>
		<li><code>[7, 4, 6]</code>: Các phần tử còn lại <code>[]</code> có phép OR bitwise bằng 0.</li>
	</ul>
	</li>
	<li>Vậy tổng số dãy con hiệu quả là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phép OR bitwise của mảng là <code>8 OR 8 = 8</code>.</li>
	<li>Chỉ có dãy con <code>[8, 8]</code> là hiệu quả vì sau khi xóa dãy con này, mảng còn lại là <code>[]</code> và có phép OR bitwise bằng 0.</li>
	<li>Vậy tổng số dãy con hiệu quả là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phép OR bitwise của mảng là <code>2 OR 2 OR 1 = 3</code>.</li>
	<li>Các dãy con hiệu quả là:
	<ul>
		<li><code>[1]</code>: Các phần tử còn lại <code>[2, 2]</code> có phép OR bitwise bằng 2.</li>
		<li><code>[2, 1]</code> (sử dụng <code>nums[0]</code>, <code>nums[2]</code>): Phần tử còn lại <code>[2]</code> có phép OR bitwise bằng 2.</li>
		<li><code>[2, 1]</code> (sử dụng <code>nums[1]</code>, <code>nums[2]</code>): Phần tử còn lại <code>[2]</code> có phép OR bitwise bằng 2.</li>
		<li><code>[2, 2]</code>: Phần tử còn lại <code>[1]</code> có phép OR bitwise bằng 1.</li>
		<li><code>[2, 2, 1]</code>: Các phần tử còn lại <code>[]</code> có phép OR bitwise bằng 0.</li>
	</ul>
	</li>
	<li>Vậy tổng số dãy con hiệu quả là 5.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con là hiệu quả khi và chỉ khi việc xóa dãy con đó làm giảm nghiêm ngặt phép OR của mảng, tức là dãy con đó chứa một bit mà không phần tử nào bên ngoài cung cấp. Với $n\le 10^5$, ta đếm số lượng giá trị cung cấp mỗi bit, sau đó đếm các dãy con mà khi xóa đi vẫn giữ được tất cả các bit, rồi lấy $2^n-1$ trừ đi kết quả đó. Đáp án được lấy modulo $10^9+7$.

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
