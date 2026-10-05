---
comments: true
difficulty: Hard
rating: 2280
source: Weekly Contest 496 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3892. Minimum Operations to Achieve At Least K Peaks](https://leetcode.com/problems/minimum-operations-to-achieve-at-least-k-peaks)

[中文文档](/solution/3800-3899/3892.Minimum%20Operations%20to%20Achieve%20At%20Least%20K%20Peaks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên vòng <code>nums</code> có độ dài <code>n</code>.</p>

<p>Một chỉ số <code>i</code> là một <strong>đỉnh</strong> nếu giá trị của nó <strong>lớn hơn nghiêm ngặt</strong> các phần tử lân cận:</p>

<ul>
	<li>Phần tử lân cận <strong>trước</strong> của <code>i</code> là <code>nums[i - 1]</code> nếu <code>i &gt; 0</code>, nếu không thì là <code>nums[n - 1]</code>.</li>
	<li>Phần tử lân cận <strong>sau</strong> của <code>i</code> là <code>nums[i + 1]</code> nếu <code>i &lt; n - 1</code>, nếu không thì là <code>nums[0]</code>.</li>
</ul>

<p>Bạn được phép thực hiện thao tác sau <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Chọn một chỉ số bất kỳ <code>i</code> và <strong>tăng</strong> <code>nums[i]</code> lên 1.</li>
</ul>

<p>Hãy trả về một số nguyên biểu thị số thao tác <strong>nhỏ nhất</strong> cần thực hiện để mảng chứa <strong>ít nhất</strong> <code>k</code> đỉnh. Nếu không thể, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,2], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Để có ít nhất <code>k = 1</code> đỉnh, ta có thể tăng <code>nums[2] = 2</code> lên 3.</li>
	<li>Sau thao tác này, <code>nums[2] = 3</code> lớn hơn nghiêm ngặt các phần tử lân cận <code>nums[0] = 2</code> và <code>nums[1] = 1</code>.</li>
	<li>Do đó, số thao tác nhỏ nhất cần thực hiện là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,5,3,6], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng đã chứa ít nhất <code>k = 2</code> đỉnh nên không cần thao tác nào.</li>
	<li>Chỉ số 1: <code>nums[1] = 5</code> lớn hơn nghiêm ngặt các phần tử lân cận <code>nums[0] = 4</code> và <code>nums[2] = 3</code>.</li>
	<li>Chỉ số 3: <code>nums[3] = 6</code> lớn hơn nghiêm ngặt các phần tử lân cận <code>nums[2] = 3</code> và <code>nums[0] = 4</code>.</li>
	<li>Do đó, số thao tác nhỏ nhất cần thực hiện là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,7,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể có ít nhất <code>k = 2</code> đỉnh trong mảng này. Do đó, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 5000</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= n</code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trên một mảng vòng, ta chỉ được cộng $1$ và cần có ít nhất $k$ đỉnh. $n \le 5000$.
>
> Các đỉnh trên mảng vòng không thể kề nhau, nên $k$ lớn có thể khiến bài toán không khả thi. Mỗi đỉnh tương ứng với việc tăng chỉ số đó sao cho giá trị của nó lớn hơn nghiêm ngặt cả hai phần tử lân cận.
>
> Giới hạn $n$ cho phép dùng $O(n^2)$ hoặc một greedy dựa trên tính chẵn lẻ: chọn một tập các chỉ số đỉnh không kề nhau; chi phí tương tác với nhau vì các ứng viên lân cận dùng chung các phần tử hai bên.
>
> Một cách là cắt mảng vòng tại một chỉ số không được chọn, sau đó dùng DP trên chuỗi thu được để tìm chi phí nhỏ nhất khi chọn ít nhất $k$ đỉnh.

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
