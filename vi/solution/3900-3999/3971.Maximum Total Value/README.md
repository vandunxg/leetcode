---
comments: true
difficulty: Hard
rating: 2194
source: Weekly Contest 507 Q4
tags:
    - Greedy
    - Array
    - Math
    - Binary Search
---

<!-- problem:start -->

# [3971. Maximum Total Value](https://leetcode.com/problems/maximum-total-value)

[中文文档](/solution/3900-3999/3971.Maximum%20Total%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>value</code> và <code>decay</code>, cùng một số nguyên <code>m</code>.</p>

<ul>
	<li><code>value[i]</code> biểu thị giá trị ban đầu tại chỉ số <code>i</code>.</li>
	<li><code>decay[i]</code> biểu thị lượng giá trị giảm đi sau mỗi lần chọn chỉ số <code>i</code>.</li>
</ul>

<p>Bạn có thể chọn bất kỳ chỉ số nào <strong>nhiều lần</strong>. Tổng số lần chọn trên tất cả các chỉ số không được vượt quá <code>m</code>.</p>

<p>Nếu bạn chọn chỉ số <code>i</code> lần thứ <code>t<sup>th</sup></code>, với <code>t</code> được đánh số từ 1, giá trị nhận được là <code>value[i] - decay[i] * (t - 1)</code>.</p>

<p>Hãy trả về tổng giá trị <strong>lớn nhất</strong> có thể nhận được. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">value = [6,5,4], decay = [2,1,1], m = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">19</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi lựa chọn tối ưu như sau:</p>

<ul>
	<li>Chọn chỉ số 0, giá trị nhận được là 6.</li>
	<li>Chọn chỉ số 1, giá trị nhận được là 5.</li>
	<li>Chọn chỉ số 2, giá trị nhận được là 4.</li>
	<li>Chọn lại chỉ số 0, giá trị nhận được là <code>6 - 2 = 4</code>.</li>
</ul>

<p>Tổng giá trị là <code>6 + 5 + 4 + 4 = 19</code>. Không có chuỗi nào khác với nhiều nhất 4 lần chọn cho giá trị lớn hơn.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">value = [7,2,2], decay = [3,2,1], m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi lựa chọn tối ưu như sau:</p>

<ul>
	<li>Chọn chỉ số 0, giá trị nhận được là 7.</li>
	<li>Chọn lại chỉ số 0, giá trị nhận được là <code>7 - 3 = 4</code>.</li>
</ul>

<p>Tổng giá trị là <code>7 + 4 = 11</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">value = [4,3], decay = [5,4], m = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi lựa chọn tối ưu như sau:</p>

<ul>
	<li>Chọn chỉ số 0, giá trị nhận được là 4.</li>
	<li>Chọn chỉ số 1, giá trị nhận được là 3.</li>
</ul>

<p>Tổng giá trị là <code>4 + 3 = 7</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= value.length == decay.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= value[i], decay[i] &lt;= 10<sup>9</sup>​​​​​​​</code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lần chọn thứ $t$ của chỉ số $i$ mang lại $\textit{value}[i]-\textit{decay}[i]\cdot(t-1)$, tạo thành một cấp số cộng giảm. $m$ lần chọn luôn nên lấy số hạng tiếp theo lớn nhất trên toàn cục.
>
> Mỗi chỉ số tạo thành một dãy giảm; heap sẽ lấy số hạng tiếp theo lớn nhất $m$ lần rồi đưa số hạng kế tiếp vào. Loại bỏ một dãy nếu số hạng tiếp theo của nó sẽ không dương.
>
> Thư mục này chưa có lời giải được cài đặt; phần trình bày dừng ở việc chọn từ nhiều dãy bằng heap.

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
