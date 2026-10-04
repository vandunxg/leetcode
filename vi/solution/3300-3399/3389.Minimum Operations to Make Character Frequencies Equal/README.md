---
comments: true
difficulty: Hard
rating: 2940
source: Weekly Contest 428 Q4
tags:
    - Hash Table
    - String
    - Dynamic Programming
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [3389. Minimum Operations to Make Character Frequencies Equal](https://leetcode.com/problems/minimum-operations-to-make-character-frequencies-equal)

[中文文档](/solution/3300-3399/3389.Minimum%20Operations%20to%20Make%20Character%20Frequencies%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code>.</p>

<p>Một chuỗi <code>t</code> được gọi là <strong>tốt</strong> nếu mọi ký tự trong <code>t</code> xuất hiện cùng số lần.</p>

<p>Bạn có thể thực hiện các thao tác sau <strong>bất kỳ số lần nào</strong>:</p>

<ul>
	<li>Xóa một ký tự khỏi <code>s</code>.</li>
	<li>Chèn một ký tự vào <code>s</code>.</li>
	<li>Đổi một ký tự trong <code>s</code> thành chữ cái kế tiếp trong bảng chữ cái.</li>
</ul>

<p><strong>Lưu ý</strong> rằng bạn không thể đổi <code>&#39;z&#39;</code> thành <code>&#39;a&#39;</code> bằng thao tác thứ ba.</p>

<p>Trả về<em> </em>số thao tác <strong>nhỏ nhất</strong> cần thực hiện để biến <code>s</code> thành một chuỗi <strong>tốt</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;acab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể làm cho <code>s</code> trở thành chuỗi tốt bằng cách xóa một lần xuất hiện của ký tự <code>&#39;a&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;wddw&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta không cần thực hiện thao tác nào vì <code>s</code> ban đầu đã là chuỗi tốt.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể làm cho <code>s</code> trở thành chuỗi tốt bằng cách thực hiện các thao tác sau:</p>

<ul>
	<li>Đổi một lần xuất hiện của <code>&#39;a&#39;</code> thành <code>&#39;b&#39;</code></li>
	<li>Chèn một lần xuất hiện của <code>&#39;c&#39;</code> vào <code>s</code></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 2&nbsp;* 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bằng cách chèn, xóa hoặc đổi chữ cái, ta muốn tần suất của mọi ký tự đều là $0$ hoặc một giá trị chung $t$. Với $|s| \le 2 \times 10^4$, ta thử từng $t$ và gán $26$ tần suất.
>
> Một phép đổi chuyển một đơn vị tần suất từ ký tự này sang ký tự khác; chèn và xóa được tính riêng. Với một $t$ cố định, ta ghép mỗi tần suất với một trong hai lựa chọn: “giữ lại $t$” hoặc “giảm về 0”.
>
> Đáp án là giá trị $t$ tốt nhất. $t$ không cần lớn hơn tần suất lớn nhất, nên số trường hợp cần duyệt là nhỏ.

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
