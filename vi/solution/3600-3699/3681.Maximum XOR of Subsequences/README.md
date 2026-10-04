---
comments: true
difficulty: Hard
rating: 2026
source: Biweekly Contest 165 Q4
tags:
    - Greedy
    - Bit Manipulation
    - Array
    - Math
---

<!-- problem:start -->

# [3681. Maximum XOR of Subsequences](https://leetcode.com/problems/maximum-xor-of-subsequences)

[中文文档](/solution/3600-3699/3681.Maximum%20XOR%20of%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó mỗi phần tử là một số nguyên không âm.</p>

<p>Chọn <strong>hai</strong> <span data-keyword="subsequence-array">subsequence</span> của <code>nums</code> (chúng có thể rỗng và <strong>được phép</strong> <strong>giao nhau</strong>), mỗi subsequence vẫn giữ nguyên thứ tự ban đầu của các phần tử, và gọi:</p>

<ul>
	<li><code>X</code> là XOR theo bit của tất cả các phần tử trong subsequence thứ nhất.</li>
	<li><code>Y</code> là XOR theo bit của tất cả các phần tử trong subsequence thứ hai.</li>
</ul>

<p>Trả về giá trị <strong>lớn nhất</strong> có thể có của <code>X XOR Y</code>.</p>

<p><strong>Lưu ý:</strong> XOR của một subsequence <strong>rỗng</strong> là 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn các subsequence:</p>

<ul>
	<li>Subsequence thứ nhất <code>[2]</code>, có XOR bằng 2.</li>
	<li>Subsequence thứ hai <code>[2,3]</code>, có XOR bằng 1.</li>
</ul>

<p>Khi đó, XOR của hai subsequence là <code>2 XOR 1 = 3</code>.</p>

<p>Đây là giá trị XOR lớn nhất có thể đạt được từ bất kỳ hai subsequence nào.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn các subsequence:</p>

<ul>
	<li>Subsequence thứ nhất <code>[5]</code>, có XOR bằng 5.</li>
	<li>Subsequence thứ hai <code>[2]</code>, có XOR bằng 2.</li>
</ul>

<p>Khi đó, XOR của hai subsequence là <code>5 XOR 2 = 7</code>.</p>

<p>Đây là giá trị XOR lớn nhất có thể đạt được từ bất kỳ hai subsequence nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> XOR của một subsequence cũng là XOR của một tập con. Giá trị lớn nhất được xác định bởi cơ sở tuyến tính: thêm từng giá trị vào, sau đó tham lam từ bit cao xuống. $n\le 10^5$ là đủ nhỏ.
>
> Duyệt từ bit cao xuống, XOR vector cơ sở vào kết quả khi nó làm tăng kết quả. Tập con rỗng có giá trị $0$ và một phần tử đơn lẻ luôn được chọn, nên kết quả không âm.
>
> Mỗi bit có nhiều nhất một vector cơ sở; thao tác thêm và truy vấn đều có độ phức tạp $O(\log A)$.

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
