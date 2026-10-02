---
comments: true
difficulty: Hard
tags:
    - Math
---

<!-- problem:start -->

# [660. Remove 9 🔒](https://leetcode.com/problems/remove-9)

[中文文档](/solution/0600-0699/0660.Remove%209/README.md)

## Mô tả

<!-- description:start -->

<p>Bắt đầu từ số nguyên <code>1</code>, loại bỏ mọi số nguyên có chứa chữ số <code>9</code>, chẳng hạn <code>9</code>, <code>19</code>, <code>29</code>...</p>

<p>Khi đó, ta có một dãy số nguyên mới <code>[1, 2, 3, 4, 5, 6, 7, 8, 10, 11, ...]</code>.</p>

<p>Cho số nguyên <code>n</code>, hãy trả về phần tử ở vị trí <code>n<sup>th</sup></code> (<strong>đánh số từ 1</strong>) trong dãy mới.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 9
<strong>Đầu ra:</strong> 10
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 11
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 8 * 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi bỏ mọi số tự nhiên có chứa chữ số $9$, cần tìm số còn lại thứ $n$. Duyệt lần lượt đến số hợp lệ thứ $n$ sẽ quá chậm khi $n$ lớn.
>
> Các số không chứa chữ số $9$ chính là các số viết trong hệ cơ số $9$ với chữ số từ $0..8$. Chuyển $n$ sang cơ số $9$. Các tab lời giải hiện vẫn để trống.

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
