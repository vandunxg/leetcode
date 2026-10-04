---
comments: true
difficulty: Hard
rating: 2222
source: Weekly Contest 466 Q4
tags:
    - Bit Manipulation
    - Math
---

<!-- problem:start -->

# [3677. Count Binary Palindromic Numbers](https://leetcode.com/problems/count-binary-palindromic-numbers)

[中文文档](/solution/3600-3699/3677.Count%20Binary%20Palindromic%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>không âm</strong> <code>n</code>.</p>

<p>Một số nguyên <strong>không âm</strong> được gọi là <strong>đối xứng nhị phân</strong> nếu biểu diễn nhị phân của nó (được viết không có các số 0 ở đầu) đọc xuôi hay ngược đều giống nhau.</p>

<p>Trả về số lượng các số nguyên <code><font face="monospace">k</font></code> thỏa mãn <code>0 &lt;= k &lt;= n</code> và biểu diễn nhị phân của <code><font face="monospace">k</font></code> là một palindrome.</p>

<p><strong>Lưu ý:</strong> Số 0 được xem là đối xứng nhị phân, và biểu diễn của nó là <code>&quot;0&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên <code>k</code> trong đoạn <code>[0, 9]</code> có biểu diễn nhị phân là palindrome là:</p>

<ul>
	<li><code>0 &rarr; &quot;0&quot;</code></li>
	<li><code>1 &rarr; &quot;1&quot;</code></li>
	<li><code>3 &rarr; &quot;11&quot;</code></li>
	<li><code>5 &rarr; &quot;101&quot;</code></li>
	<li><code>7 &rarr; &quot;111&quot;</code></li>
	<li><code>9 &rarr; &quot;1001&quot;</code></li>
</ul>

<p>Tất cả các giá trị khác trong <code>[0, 9]</code> đều có dạng nhị phân không phải palindrome. Vì vậy, số lượng là 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>&quot;0&quot;</code> là một palindrome, số lượng là 1.</p>
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
> Đếm các palindrome nhị phân trong $[0,n]$. Vì $n$ lớn, ta sinh một palindrome từ nửa đầu của nó.
>
> Gọi độ dài bit của $n$ là $L$. Các palindrome ngắn hơn $L$ được đếm theo độ dài; những palindrome có độ dài $L$ được tạo từ các nửa đầu mà khi đối xứng lại có giá trị không lớn hơn $n$.
>
> Với độ dài lẻ, có một bit ở giữa được tự do chọn. Xem nửa đầu là một số nguyên, đối xứng nó, so sánh với $n$, đồng thời cộng tất cả các độ dài nhỏ hơn.

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
