---
comments: true
difficulty: Hard
rating: 2370
source: Weekly Contest 411 Q3
tags:
    - Greedy
    - Math
    - String
    - Dynamic Programming
    - Number Theory
---

<!-- problem:start -->

# [3260. Find the Largest Palindrome Divisible by K](https://leetcode.com/problems/find-the-largest-palindrome-divisible-by-k)

[中文文档](/solution/3200-3299/3260.Find%20the%20Largest%20Palindrome%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <strong>dương</strong> <code>n</code> và <code>k</code>.</p>

<p>Một số nguyên <code>x</code> được gọi là <strong>k-palindromic</strong> nếu:</p>

<ul>
	<li><code>x</code> là một <span data-keyword="palindrome-integer">số đối xứng</span>.</li>
	<li><code>x</code> chia hết cho <code>k</code>.</li>
</ul>

<p>Hãy trả về<strong> số nguyên lớn nhất</strong> có <code>n</code> chữ số (dưới dạng chuỗi) và là <strong>k-palindromic</strong>.</p>

<p><strong>Lưu ý</strong> rằng số nguyên này <strong>không được</strong> có các số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;595&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>595 là số nguyên k-palindromic lớn nhất có 3 chữ số.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;8&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>4 và 8 là hai số nguyên k-palindromic duy nhất có 1 chữ số.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;89898&quot;</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xây dựng số đối xứng lớn nhất có $n$ chữ số và chia hết cho $k$, với $n\le 10^5$ và $k\le 9$. Liệt kê các số đối xứng theo thứ tự giảm dần là bất khả thi; nửa đầu quyết định phần còn lại, và ta chỉ cần quan tâm đến giá trị modulo $k$.
>
> Xét từng trường hợp của $k$ (các chữ số cuối với $2,4,5,8$, tổng chữ số với $3,9$, cả hai với $6,7$), tham lam điền các chữ số 9 rồi sửa các vị trí thấp nhất để toàn bộ số có giá trị $0\bmod k$. Hiện chưa có phần cài đặt trong cây; ý tưởng là “cố định nửa đầu, rồi điều chỉnh phần đuôi cho $k$”.

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
