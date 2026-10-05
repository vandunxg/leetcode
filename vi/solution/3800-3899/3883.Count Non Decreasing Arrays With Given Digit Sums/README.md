---
comments: true
difficulty: Hard
rating: 2172
source: Biweekly Contest 179 Q4
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3883. Count Non Decreasing Arrays With Given Digit Sums](https://leetcode.com/problems/count-non-decreasing-arrays-with-given-digit-sums)

[中文文档](/solution/3800-3899/3883.Count%20Non%20Decreasing%20Arrays%20With%20Given%20Digit%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>digitSum</code> có độ dài <code>n</code>.</p>

<p>Một mảng <code>arr</code> có độ dài <code>n</code> được gọi là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li><code>0 &lt;= arr[i] &lt;= 5000</code></li>
	<li>là <strong>không giảm</strong>.</li>
	<li><strong>tổng các chữ số</strong> của <code>arr[i]</code> <strong>bằng</strong> <code>digitSum[i]</code>.</li>
</ul>

<p>Trả về một số nguyên biểu thị số lượng <strong>mảng hợp lệ khác nhau</strong>. Vì đáp án có thể rất lớn, hãy trả về đáp án theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>Một mảng được gọi là <strong>không giảm</strong> nếu mỗi phần tử lớn hơn hoặc bằng phần tử liền trước, nếu phần tử đó tồn tại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">digitSum = [25,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số có tổng chữ số bằng 25 là 799, 889, 898, 979, 988 và 997.</p>

<p>Số duy nhất có tổng chữ số bằng 1 có thể xuất hiện sau các giá trị này mà vẫn giữ cho mảng không giảm là 1000.</p>

<p>Vì vậy, các mảng hợp lệ là <code>[799, 1000]</code>, <code>[889, 1000]</code>, <code>[898, 1000]</code>, <code>[979, 1000]</code>, <code>[988, 1000]</code> và <code>[997, 1000]</code>.</p>

<p>Do đó, đáp án là 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">digitSum = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng hợp lệ là <code>[1]</code>, <code>[10]</code>, <code>[100]</code> và <code>[1000]</code>.</p>

<p>Do đó, đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">digitSum = [2,49,23]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nguyên nào trong khoảng [0, 5000] có tổng chữ số bằng 49. Vì vậy, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= digitSum.length &lt;= 1000</code></li>
	<li><code>0 &lt;= digitSum[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các mảng không giảm với $0 \le arr[i] \le 5000$ sao cho tổng các chữ số bằng $\textit{digitSum}[i]$. $n \le 1000$ và tổng các chữ số $\le 50$.
>
> Tính đơn điệu biến bài toán thành việc chọn một giá trị tại mỗi chỉ số, sao cho giá trị đó không nhỏ hơn giá trị trước đó. Mỗi tổng chữ số có hữu hạn giá trị ứng viên.
>
> Tính trước các số hợp lệ theo từng tổng, sau đó dùng DP trên chỉ số và giá trị cuối, chuyển sang một ứng viên lớn hơn hoặc bằng giá trị đó.
>
> Lấy modulo $10^9+7$. Các giá trị tối đa là $5000$, nên ta sắp xếp các ứng viên và dùng prefix sum để tăng tốc các chuyển trạng thái.

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
