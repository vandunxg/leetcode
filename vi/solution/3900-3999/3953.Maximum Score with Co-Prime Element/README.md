---
comments: true
difficulty: Hard
rating: 2390
source: Biweekly Contest 184 Q4
tags:
    - Array
    - Hash Table
    - Math
    - Combinatorics
    - Enumeration
    - Number Theory
---

<!-- problem:start -->

# [3953. Maximum Score with Co-Prime Element](https://leetcode.com/problems/maximum-score-with-co-prime-element)

[Tài liệu tiếng Trung](/solution/3900-3999/3953.Maximum%20Score%20with%20Co-Prime%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>maxVal</code>.</p>

<p>Bạn <strong>có thể</strong> đổi bất kỳ phần tử nào trong <code>nums</code> thành một số nguyên dương bất kỳ <strong>nhỏ hơn hoặc bằng</strong> <code>maxVal</code>. Mỗi lần đổi có chi phí là 1.</p>

<p>Hai số nguyên được gọi là <strong>nguyên tố cùng nhau</strong> nếu <span data-keyword="gcd-function"><strong>ước chung lớn nhất (GCD)</strong></span> của chúng bằng 1.</p>

<p>Sau tất cả các lần thay đổi, bạn <strong>phải</strong> chọn một chỉ số <code>i</code> sao cho <code>nums[i]</code> <strong>nguyên tố cùng nhau</strong> với mọi phần tử <code>nums[j]</code> khác.</p>

<p>Gọi:</p>

<ul>
	<li><code>selectedValue</code> là giá trị cuối cùng của <code>nums[i]</code> sau các lần thay đổi.</li>
	<li><code>modificationCost</code> là tổng số phần tử đã bị thay đổi.</li>
</ul>

<p>Điểm số được định nghĩa là <code>score = selectedValue - modificationCost</code>.</p>

<p>Trả về <strong>điểm số lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,4,6], maxVal = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đổi <code>nums[2]</code> từ 6 thành 5, với chi phí là 1. Chọn <code>nums[2] = 5</code>, vì nó nguyên tố cùng nhau với 3 và 4.</p>

<ul>
	<li><code>selectedValue = 5</code></li>
	<li><code>modificationCost = 1</code></li>
	<li>Điểm số là <code>5 - 1 = 4</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], maxVal = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không cần thay đổi. Chọn <code>nums[2] = 3</code>, vì nó nguyên tố cùng nhau với 1 và 2.</p>

<ul>
	<li><code>selectedValue = 3</code></li>
	<li><code>modificationCost = 0</code></li>
	<li>Điểm số là <code>3 - 0 = 3</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2], maxVal = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đổi <code>nums[0]</code> từ 2 thành 1, với chi phí là 1. Chọn <code>nums[1] = 2</code>, vì nó nguyên tố cùng nhau với 1.</p>

<ul>
	<li><code>selectedValue = 2</code></li>
	<li><code>modificationCost = 1</code></li>
	<li>Điểm số là ​​​​​​​<code>2 - 1 = 1</code></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maxVal &lt;= 10<sup>​​​​​​​5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi chọn $v$, mọi phần tử không nguyên tố cùng nhau với $v$ đều phải bị thay đổi; điểm số bằng $v$ trừ đi số phần tử đó. Vì $v\le\textit{maxVal}$, ta cần biết có bao nhiêu giá trị trong mảng có chung một thừa số nguyên tố với $v$.
>
> Phân tích mảng theo các ước nguyên tố nhỏ nhất hoặc theo số lần xuất hiện của các bội, sau đó dùng nguyên lý bù trừ trên $v$ để tính số phần tử không nguyên tố cùng nhau. Việc có duyệt mọi giá trị đến $\textit{maxVal}$ hay không phụ thuộc vào giới hạn đó.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần tư duy dừng ở việc liệt kê $v$ và đếm số phần tử không nguyên tố cùng nhau.

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
