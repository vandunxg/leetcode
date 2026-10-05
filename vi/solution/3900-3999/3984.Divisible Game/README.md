---
comments: true
difficulty: Medium
rating: 1944
source: Weekly Contest 509 Q3
tags:
    - Array
    - Math
    - Dynamic Programming
    - Enumeration
    - Number Theory
---

<!-- problem:start -->

# [3984. Divisible Game](https://leetcode.com/problems/divisible-game)

[中文文档](/solution/3900-3999/3984.Divisible%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Alice và Bob đang chơi một trò chơi. Alice chọn:</p>

<ul>
	<li>Một số nguyên <code>k</code> sao cho <code>k &gt; 1</code>.</li>
	<li>Hai số nguyên <code>l</code> và <code>r</code> sao cho <code>0 &lt;= l &lt;= r &lt; n</code>.</li>
</ul>

<p>Ban đầu, điểm của Alice và Bob đều bằng 0.</p>

<p>Với mỗi chỉ số <code>i</code> trong đoạn <code>[l, r]</code> (bao gồm cả hai đầu mút):</p>

<ul>
	<li>Nếu <code>nums[i]</code> chia hết cho <code>k</code>, điểm của Alice <strong>tăng</strong> thêm <code>nums[i]</code>.</li>
	<li>Ngược lại, điểm của Bob <strong>tăng</strong> thêm <code>nums[i]</code>.</li>
</ul>

<p><strong>Hiệu điểm</strong> là điểm của Alice <strong>trừ</strong> điểm của Bob.</p>

<p>Alice muốn <strong>tối đa hóa</strong> hiệu điểm. Nếu có nhiều giá trị <code>k</code> đạt được hiệu điểm <strong>lớn nhất</strong>, cô ấy chọn giá trị <code>k</code> <strong>nhỏ nhất</strong>.</p>

<p>Trả về <strong>tích</strong> của hiệu điểm <strong>lớn nhất</strong> và giá trị <code>k</code> đã chọn. Vì kết quả có thể rất lớn, hãy trả về phần dư khi chia <strong>modulo</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,6,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">36</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Alice có thể chọn <code>k = 2</code>, <code>l = 1</code> và <code>r = 3</code>.</li>
	<li>Mọi giá trị trong <code>nums[1..3]</code> đều chia hết cho 2, nên điểm của Alice là <code>4 + 6 + 8 = 18</code>, còn điểm của Bob là 0.</li>
	<li>Hiệu điểm là 18, đây là giá trị lớn nhất có thể đạt được. Trong các giá trị <code>k</code> đạt hiệu điểm này, giá trị nhỏ nhất là 2.</li>
	<li>Vì vậy, đáp án là <code>18 * 2 = 36</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Alice có thể chọn <code>k = 2</code>, <code>l = 0</code> và <code>r = 2</code>.</li>
	<li>Các giá trị <code>nums[0]</code> và <code>nums[2]</code> chia hết cho 2, nên điểm của Alice là <code>2 + 2 = 4</code>. Giá trị <code>nums[1]</code> không chia hết cho 2, nên điểm của Bob là 1.</li>
	<li>Hiệu điểm là <code>4 - 1 = 3</code>, đây là giá trị lớn nhất có thể đạt được. Trong các giá trị <code>k</code> đạt hiệu điểm này, giá trị nhỏ nhất là 2.</li>
	<li>Vì vậy, đáp án là <code>3 * 2 = 6</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1000000005</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Alice phải chọn một <code>k &gt; 1</code>. Giá trị nhỏ nhất có thể chọn là <code>k = 2</code>.</li>
	<li>Vì <code>nums[0]</code> không chia hết cho 2, điểm của Alice là 0, còn điểm của Bob là 1.</li>
	<li>Hiệu điểm là -1, đây là giá trị lớn nhất có thể đạt được.</li>
	<li>Vì vậy, đáp án là <code>-1 * 2 = -2</code>. Lấy phần dư theo <code>10<sup>9</sup> + 7</code>, kết quả là 1000000005.</li>
</ul>
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
> Alice chọn $k>1$ và một đoạn con; các bội của $k$ được cộng cho cô ấy, còn các số khác được cộng cho Bob. Hiệu điểm bằng $2\cdot(\text{sum of multiples})-\text{subarray sum}$.
>
> Với mỗi $k$, các vị trí chứa bội tạo thành những đoạn liên tiếp, và tổng tiền tố của chúng cho ta hiệu điểm tốt nhất. Trong các $k$ đạt hiệu điểm lớn nhất, chọn giá trị nhỏ nhất rồi nhân với hiệu điểm.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng ở việc nhóm các bội theo $k$.

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
