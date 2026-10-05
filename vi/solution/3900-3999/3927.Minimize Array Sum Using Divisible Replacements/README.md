---
comments: true
difficulty: Medium
rating: 1651
source: Weekly Contest 501 Q3
tags:
    - Greedy
    - Array
    - Hash Table
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3927. Minimize Array Sum Using Divisible Replacements](https://leetcode.com/problems/minimize-array-sum-using-divisible-replacements)

[中文文档](/solution/3900-3999/3927.Minimize%20Array%20Sum%20Using%20Divisible%20Replacements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn có thể thực hiện thao tác sau một số lần bất kỳ:</p>

<ul>
	<li>Chọn hai chỉ số <code>a</code> và <code>b</code> sao cho <code>nums[a] % nums[b] == 0</code>.</li>
	<li>Thay <code>nums[a]</code> bằng <code>nums[b]</code>.</li>
</ul>

<p>Trả về tổng <strong>nhỏ nhất</strong> có thể của mảng sau khi thực hiện một số thao tác bất kỳ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,6,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>a = 1</code>, <code>b = 2</code>, trong đó <code>nums[a] = 6</code> và <code>nums[b] = 2</code>. Vì <code>6 % 2 == 0</code>, thay <code>nums[1]</code> bằng <code>nums[2]</code>.</li>
	<li>Mảng trở thành <code>[3, 2, 2]</code>.</li>
	<li>Không còn thao tác nào làm giảm tổng. Do đó, tổng cuối cùng là <code>3 + 2 + 2 = 7</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,8,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>a = 0</code>, <code>b = 1</code>, trong đó <code>nums[a] = 4</code> và <code>nums[b] = 2</code>. Vì <code>4 % 2 == 0</code>, thay <code>nums[0]</code> bằng <code>nums[1]</code>.</li>
	<li>Chọn <code>a = 2</code>, <code>b = 1</code>, trong đó <code>nums[a] = 8</code> và <code>nums[b] = 2</code>. Vì <code>8 % 2 == 0</code>, thay <code>nums[2]</code> bằng <code>nums[1]</code>.</li>
	<li>Mảng trở thành <code>[2, 2, 2, 3]</code>.</li>
	<li>Không còn thao tác nào làm giảm tổng. Do đó, tổng cuối cùng là <code>2 + 2 + 2 + 3 = 9</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,5,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">21</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không tồn tại cặp <code>(a, b)</code> nào sao cho <code>nums[a] % nums[b] == 0</code>.</li>
	<li>Vì vậy, không thể thực hiện thao tác nào. Tổng vẫn là <code>7 + 5 + 9 = 21</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>​​​​​​​5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 10^5$, ta không thể mô phỏng các phép thay thế tùy ý. Một phần tử $a$ có thể bị thay thế bởi bất kỳ $b$ nào chia hết cho nó, và lặp lại thao tác này sẽ cho giá trị nhỏ nhất trong mảng có thể chia hết cho $a$.
>
> Trên toàn mảng, mỗi số nên trở thành phần tử nhỏ nhất của mảng chia hết cho nó. Nếu $m$ là giá trị nhỏ nhất toàn cục, mọi bội của $m$ đều có thể trở thành $m$, còn các phần tử khác giữ nguyên; đáp án là tổng các giá trị cuối cùng đó.
>
> Thư mục này hiện chưa có lời giải được triển khai; phần trình bày dừng ở việc tìm ước nhỏ nhất trong mảng cho từng vị trí.

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
