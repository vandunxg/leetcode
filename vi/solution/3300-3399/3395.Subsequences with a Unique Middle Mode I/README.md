---
comments: true
difficulty: Hard
rating: 2799
source: Biweekly Contest 146 Q4
tags:
    - Array
    - Hash Table
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [3395. Subsequences with a Unique Middle Mode I](https://leetcode.com/problems/subsequences-with-a-unique-middle-mode-i)

[中文文档](/solution/3300-3399/3395.Subsequences%20with%20a%20Unique%20Middle%20Mode%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy tìm số lượng <span data-keyword="subsequence-array">dãy con</span> độ dài 5 của <code>nums</code> có <strong>mode giữa duy nhất</strong>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Mode</strong> của một dãy số được định nghĩa là phần tử xuất hiện với số lần <strong>lớn nhất</strong> trong dãy.</p>

<p>Một dãy số có <strong>mode duy nhất</strong> nếu nó chỉ có một mode.</p>

<p>Một dãy số <code>seq</code> có độ dài 5 có <strong>mode giữa duy nhất</strong> nếu <em>phần tử ở giữa</em> (<code>seq[2]</code>) là một <strong>mode duy nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>[1, 1, 1, 1, 1]</code> là dãy con độ dài 5 duy nhất có thể tạo ra và có mode giữa duy nhất là 1. Có thể tạo dãy con này theo 6 cách khác nhau, nên kết quả là 6.&nbsp;</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,3,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>[1, 2, 2, 3, 4]</code> và <code>[1, 2, 3, 3, 4]</code> đều có mode giữa duy nhất vì phần tử ở chỉ số 2 có tần suất lớn nhất trong dãy con. <code>[1, 2, 2, 3, 3]</code> không có mode giữa duy nhất vì 2 và 3 đều xuất hiện hai lần.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,2,3,4,5,6,7,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có dãy con độ dài 5 nào có mode giữa duy nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>5 &lt;= nums.length &lt;= 1000</code></li>
	<li><code><font face="monospace">-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con độ dài-$5$ được tính khi giá trị ở giữa là mode duy nhất. Với $n \le 1000$, ta cố định chỉ số ở giữa rồi chọn hai chỉ số ở mỗi phía.
>
> Duyệt thô với độ phức tạp $O(n^5)$ không đáp ứng được. Sau khi cố định $x=\textit{nums}[i]$, ta phân loại tần suất ở bên trái/bên phải để đảm bảo $x$ xuất hiện nhiều hơn mọi giá trị khác.
>
> Kết hợp các bảng tần suất bên trái và bên phải, đồng thời loại bỏ các trường hợp $x$ bị hòa hoặc không phải mode. Với các map tiền tố $O(n)$ tại $i$, mỗi vị trí giữa được xử lý trong $O(1)$ hoặc $O(|\Sigma|)$.

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
