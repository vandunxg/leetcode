---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [3416. Subsequences with a Unique Middle Mode II 🔒](https://leetcode.com/problems/subsequences-with-a-unique-middle-mode-ii)

[中文文档](/solution/3400-3499/3416.Subsequences%20with%20a%20Unique%20Middle%20Mode%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy tìm số lượng <span data-keyword="subsequence-array">dãy con</span> có độ dài 5 của&nbsp;<code>nums</code> có <strong>mode giữa duy nhất</strong>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Mode</strong> của một dãy số là phần tử xuất hiện <strong>nhiều nhất</strong> trong dãy.</p>

<p>Một dãy số chứa <strong>mode duy nhất</strong> nếu nó chỉ có một mode.</p>

<p>Một dãy số <code>seq</code> có độ dài 5 chứa <strong>mode giữa duy nhất</strong> nếu <em>phần tử ở giữa</em> (<code>seq[2]</code>) là một <strong>mode duy nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<p><strong>Đầu vào:</strong> nums = [1,1,1,1,1,1]</p>

<p><strong>Đầu ra:</strong> 6</p>

<p><strong>Giải thích:</strong></p>

<p><code>[1, 1, 1, 1, 1]</code> là dãy con duy nhất có độ dài 5 có thể tạo được từ danh sách này, và nó có mode giữa duy nhất là 1.</p>

<p><strong>Ví dụ 2:</strong></p>

<p><strong>Đầu vào:</strong> nums = [1,2,2,3,3,4]</p>

<p><strong>Đầu ra:</strong> 4</p>

<p><strong>Giải thích:</strong></p>

<p><code>[1, 2, 2, 3, 4]</code> và <code>[1, 2, 3, 3, 4]</code> có mode giữa duy nhất vì phần tử ở chỉ số 2 có tần suất lớn nhất trong dãy con. <code>[1, 2, 2, 3, 3]</code> không có mode giữa duy nhất vì cả 2 và 3 đều xuất hiện hai lần trong dãy con.</p>

<p><strong>Ví dụ 3:</strong></p>

<p><strong>Đầu vào:</strong> nums = [0,1,2,3,4,5,6,7,8]</p>

<p><strong>Đầu ra:</strong> 0</p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại dãy con độ dài 5 có mode giữa duy nhất.</p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>5 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta đếm các dãy con độ dài-$5$ mà giá trị ở giữa là mode duy nhất. $n\le 10^5$ khiến việc liệt kê $C(n,5)$ là không thể.
>
> Phần tử ở giữa trong năm vị trí phải xuất hiện nhiều lần hơn mọi giá trị khác. Cố định chỉ số trung tâm $i$ giúp bài toán trở thành chọn hai chỉ số ở mỗi phía.
>
> Các frequency map ở bên trái và bên phải phân loại các trường hợp dựa trên số lần giá trị trung tâm xuất hiện ($3/4/5$) và việc có giá trị khác xuất hiện với cùng tần suất hay không. Dùng nguyên lý bù hoặc phép bao hàm để trừ các trường hợp không hợp lệ. Chỉ cần duyệt một lần qua $i$ nên độ phức tạp gần tuyến tính-logarit.

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
