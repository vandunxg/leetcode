---
comments: true
difficulty: Hard
rating: 2445
source: Weekly Contest 462 Q4
tags:
    - Bit Manipulation
    - Backtracking
---

<!-- problem:start -->

# [3646. Next Special Palindrome Number](https://leetcode.com/problems/next-special-palindrome-number)

[中文文档](/solution/3600-3699/3646.Next%20Special%20Palindrome%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>.</p>

<p>Một số được gọi là <strong>đặc biệt</strong> nếu:</p>

<ul>
	<li>Nó là một số <strong><span data-keyword="palindrome-integer">đối xứng</span></strong>.</li>
	<li>Mỗi chữ số <code>k</code> trong số đó xuất hiện <strong>đúng</strong> <code>k</code> lần.</li>
</ul>

<p>Trả về số đặc biệt <strong>nhỏ nhất</strong> <strong>lớn hơn nghiêm ngặt</strong> <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<p>22 là số đặc biệt nhỏ nhất lớn hơn 2, vì nó là số đối xứng và chữ số 2 xuất hiện đúng 2 lần.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 33</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">212</span></p>

<p><strong>Giải thích:</strong></p>

<p>212 là số đặc biệt nhỏ nhất lớn hơn 33, vì nó là số đối xứng và các chữ số 1 và 2 lần lượt xuất hiện đúng 1 và 2 lần.<br />
  </p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một số đối xứng đặc biệt sử dụng chữ số $d$ đúng $d$ lần theo quy tắc tần suất đã cho và đọc xuôi hay ngược đều giống nhau. Việc kiểm tra lần lượt $n+1,n+2,\ldots$ sẽ không phù hợp khi $n$ lớn.
>
> Chỉ có hữu hạn multiset thỏa mãn các tần suất này. Ta liệt kê các hoán vị của một nửa, đối xứng chúng, sắp xếp rồi tìm kiếm nhị phân số đứng sau $n$.
>
> Có nhiều nhất một chữ số có số lần xuất hiện lẻ nằm ở giữa; các chữ số còn lại xuất hiện theo từng cặp. Sau khi tạo mọi ứng viên, với mỗi truy vấn, ta chỉ cần tìm giá trị nhỏ nhất nghiêm ngặt lớn hơn $n$.

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
