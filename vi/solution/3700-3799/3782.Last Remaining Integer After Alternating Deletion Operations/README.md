---
comments: true
difficulty: Hard
rating: 2074
source: Biweekly Contest 172 Q4
tags:
    - Recursion
    - Math
---

<!-- problem:start -->

# [3782. Last Remaining Integer After Alternating Deletion Operations](https://leetcode.com/problems/last-remaining-integer-after-alternating-deletion-operations)

[中文文档](/solution/3700-3799/3782.Last%20Remaining%20Integer%20After%20Alternating%20Deletion%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Ta viết các số nguyên từ 1 đến <code>n</code> thành một dãy từ trái sang phải. Sau đó, <strong>lần lượt</strong> áp dụng hai thao tác sau cho đến khi chỉ còn lại một số nguyên, bắt đầu bằng thao tác 1:</p>

<ul>
	<li><strong>Thao tác 1</strong>: Bắt đầu từ bên trái, xóa mọi số thứ hai.</li>
	<li><strong>Thao tác 2</strong>: Bắt đầu từ bên phải, xóa mọi số thứ hai.</li>
</ul>

<p>Trả về số nguyên còn lại cuối cùng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Viết <code>[1, 2, 3, 4, 5, 6, 7, 8]</code> thành một dãy.</li>
	<li>Bắt đầu từ bên trái, ta xóa mọi số thứ hai: <code>[1, <u><strong>2</strong></u>, 3, <u><strong>4</strong></u>, 5, <u><strong>6</strong></u>, 7, <u><strong>8</strong></u>]</code>. Các số nguyên còn lại là <code>[1, 3, 5, 7]</code>.</li>
	<li>Bắt đầu từ bên phải, ta xóa mọi số thứ hai: <code>[<u><strong>1</strong></u>, 3, <u><strong>5</strong></u>, 7]</code>. Các số nguyên còn lại là <code>[3, 7]</code>.</li>
	<li>Bắt đầu từ bên trái, ta xóa mọi số thứ hai: <code>[3, <u><strong>7</strong></u>]</code>. Số nguyên còn lại là <code>[3]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Viết <code>[1, 2, 3, 4, 5]</code> thành một dãy.</li>
	<li>Bắt đầu từ bên trái, ta xóa mọi số thứ hai: <code>[1, <u><strong>2</strong></u>, 3, <u><strong>4</strong></u>, 5]</code>. Các số nguyên còn lại là <code>[1, 3, 5]</code>.</li>
	<li>Bắt đầu từ bên phải, ta xóa mọi số thứ hai: <code>[1, <u><strong>3</strong></u>, 5]</code>. Các số nguyên còn lại là <code>[1, 5]</code>.</li>
	<li>Bắt đầu từ bên trái, ta xóa mọi số thứ hai: <code>[1, <u><strong>5</strong></u>]</code>. Số nguyên còn lại là <code>[1]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Viết <code>[1]</code> thành một dãy.</li>
	<li>Số nguyên còn lại cuối cùng là 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n$ có thể bằng $10^{15}$, vì vậy ta không thể mô phỏng các lần xóa. Đây là một biến thể của bài toán Josephus, trong đó lần lượt xóa mọi số thứ hai từ bên trái rồi từ bên phải. Ta duy trì số hạng đầu tiên và công sai của cấp số cộng gồm các phần tử còn lại cho đến khi chỉ còn một giá trị.

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
