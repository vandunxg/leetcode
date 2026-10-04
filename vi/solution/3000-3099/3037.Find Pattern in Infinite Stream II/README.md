---
comments: true
difficulty: Hard
tags:
    - Array
    - Interactive
    - String Matching
    - Sliding Window
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3037. Find Pattern in Infinite Stream II 🔒](https://leetcode.com/problems/find-pattern-in-infinite-stream-ii)

[中文文档](/solution/3000-3099/3037.Find%20Pattern%20in%20Infinite%20Stream%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng nhị phân <code>pattern</code> và một đối tượng <code>stream</code> thuộc lớp <code>InfiniteStream</code>, biểu diễn một stream vô hạn các bit được đánh chỉ số từ <strong>0</strong>.</p>

<p>Lớp <code>InfiniteStream</code> có hàm sau:</p>

<ul>
	<li><code>int next()</code>: Đọc một bit <strong>duy nhất</strong> (là <code>0</code> hoặc <code>1</code>) từ stream và trả về bit đó.</li>
</ul>

<p>Trả về <em><strong>chỉ số bắt đầu đầu tiên</strong> mà tại đó pattern khớp với các bit được đọc từ stream</em>. Ví dụ, nếu pattern là <code>[1, 0]</code>, kết quả khớp đầu tiên là phần được đánh dấu trong stream <code>[0, <strong><u>1, 0</u></strong>, 1, ...]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stream = [1,1,1,0,1,1,1,...], pattern = [0,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Lần xuất hiện đầu tiên của pattern [0,1] được đánh dấu trong stream [1,1,1,<strong><u>0,1</u></strong>,...], bắt đầu tại chỉ số 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stream = [0,0,0,0,...], pattern = [0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Lần xuất hiện đầu tiên của pattern [0] được đánh dấu trong stream [<strong><u>0</u></strong>,...], bắt đầu tại chỉ số 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> stream = [1,0,1,1,0,1,1,0,1,...], pattern = [1,1,0,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Lần xuất hiện đầu tiên của pattern [1,1,0,1] được đánh dấu trong stream [1,0,<strong><u>1,1,0,1</u></strong>,...], bắt đầu tại chỉ số 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt;= 10<sup>4</sup></code></li>
	<li><code>pattern</code> chỉ gồm các giá trị <code>0</code> và <code>1</code>.</li>
	<li><code>stream</code> chỉ gồm các giá trị <code>0</code> và <code>1</code>.</li>
	<li>Đầu vào được tạo sao cho chỉ số bắt đầu của pattern tồn tại trong <code>10<sup>5</sup></code> bit đầu tiên của stream.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> pattern có thể dài tới $10^4$, nên việc đóng gói nó vào hai word $64$-bit như ở phần I không còn khả thi. stream vẫn chỉ có thể đọc.
>
> Đây là bài toán khớp mẫu trên stream thông thường: hàm failure của KMP chỉ phụ thuộc vào pattern, nên mỗi bit có thể cập nhật trạng thái mà không cần đọc lùi stream.
>
> Ta xây dựng hàm prefix của $\textit{pattern}$ và duy trì độ dài phần đã khớp hiện tại; khi độ dài này đạt bằng độ dài pattern, ta trả về chỉ số bắt đầu.

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
