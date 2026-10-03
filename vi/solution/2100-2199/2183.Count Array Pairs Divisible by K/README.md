---
comments: true
difficulty: Hard
rating: 2246
source: Weekly Contest 281 Q4
tags:
    - Array
    - Hash Table
    - Math
    - Counting
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2183. Count Array Pairs Divisible by K](https://leetcode.com/problems/count-array-pairs-divisible-by-k)

[中文文档](/solution/2100-2199/2183.Count%20Array%20Pairs%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>, hãy trả về <em><strong>số lượng cặp</strong></em> <code>(i, j)</code> <em>thỏa mãn:</em></p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt;= n - 1</code> <em>và</em></li>
	<li><code>nums[i] * nums[j]</code> <em>chia hết cho</em> <code>k</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5], k = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
7 cặp chỉ số có tích tương ứng chia hết cho 2 là
(0, 1), (0, 3), (1, 2), (1, 3), (1, 4), (2, 3) và (3, 4).
Tích của chúng lần lượt là 2, 4, 6, 8, 10, 12 và 20.
Các cặp khác như (0, 2) và (2, 4) có tích lần lượt là 3 và 15, không chia hết cho 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], k = 5
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không tồn tại cặp chỉ số nào có tích tương ứng chia hết cho 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp có tích chia hết cho $k$. Vì $n\le 10^5$, ta không thể dùng vòng lặp đôi. Việc $a\cdot b$ có đồng dư $0$ modulo $k$ hay không chỉ phụ thuộc vào việc $\gcd(a,k)$ và $\gcd(b,k)$ có bao phủ đầy đủ mọi lũy thừa của các số nguyên tố trong $k$ hay không.
>
> Thay mỗi giá trị bằng $\gcd(x,k)$; các giá trị phân biệt thu được chính là các ước của $k$. Sau khi đếm các giá trị gcd đó, ta liệt kê các cặp ước $(a,b)$ có tích là bội của $k$ rồi kết hợp tần suất xuất hiện.
>
> Chỉ cần một hash map lưu tần suất gcd và một vòng lặp đôi trên các ước là đủ. Các tab code để trống; phần trình bày này đi theo cách đếm bằng số học đó.

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
