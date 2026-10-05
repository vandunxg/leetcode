---
comments: true
difficulty: Hard
rating: 2201
source: Weekly Contest 509 Q4
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Hash Function
---

<!-- problem:start -->

# [3985. Palindromic Subarray Sum](https://leetcode.com/problems/palindromic-subarray-sum)

[中文文档](/solution/3900-3999/3985.Palindromic%20Subarray%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Trả về tổng lớn nhất có thể của một <span data-keyword="subarray-nonempty">mảng con</span> của <code>nums</code> là một <span data-keyword="palindrome-array">palindrome</span>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<p>Toàn bộ mảng <code>[10,10]</code> là một palindrome. Vì vậy, tổng lớn nhất là <code>10 + 10 = 20</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,2,1,5,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con liên tiếp <code>[1,2,3,2,1]</code> là một palindrome. Tổng của nó là <code>1 + 2 + 3 + 2 + 1 = 9</code>, và đây là tổng lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,1,2,1,7,3,4,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">18</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con liên tiếp <code>[7,1,2,1,7]</code> là một palindrome. Tổng của nó là <code>7 + 1 + 2 + 1 + 7 = 18</code>, và đây là tổng lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có mảng con nào có độ dài lớn hơn 1 là palindrome. Phần tử lớn nhất trong mảng là 5. Vì vậy, đáp án là 5.</p>
</div>

<p><strong class="example">Ví dụ 5:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1000]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1000</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con chỉ có một phần tử là một palindrome. Vì vậy, đáp án là 1000.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>​​​​​​​9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^5$, nên việc mở rộng quanh tâm của mọi palindrome có độ phức tạp $O(n^2)$ và có thể quá chậm. Mọi mảng con dạng palindrome hoặc là một phần tử đơn (do đó đáp án ít nhất là $\max\textit{nums}$), hoặc là kết quả mở rộng các cặp phần tử bằng nhau quanh một tâm.
>
> Nếu các giá trị không lặp lại quá thường xuyên, ta vẫn có thể dùng cách mở rộng quanh tâm; nếu không, cần hash các phép so sánh bằng nhau hoặc xử lý bằng palindromic automaton. Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng ở việc so sánh mở rộng quanh tâm với giá trị lớn nhất của palindrome một phần tử.

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
