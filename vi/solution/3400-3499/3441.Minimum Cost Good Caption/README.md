---
comments: true
difficulty: Hard
rating: 2764
source: Biweekly Contest 149 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3441. Minimum Cost Good Caption](https://leetcode.com/problems/minimum-cost-good-caption)

[Tài liệu tiếng Trung](/solution/3400-3499/3441.Minimum%20Cost%20Good%20Caption/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>caption</code> có độ dài <code>n</code>. Một caption <strong>hợp lệ</strong> là chuỗi trong đó <strong>mọi</strong> ký tự xuất hiện thành các nhóm gồm <strong>ít nhất 3</strong> lần liên tiếp.</p>

<p>Ví dụ:</p>

<ul>
	<li><code>&quot;aaabbb&quot;</code> và <code>&quot;aaaaccc&quot;</code> là các caption <strong>hợp lệ</strong>.</li>
	<li><code>&quot;aabbb&quot;</code> và <code>&quot;ccccd&quot;</code> không phải là caption <strong>hợp lệ</strong>.</li>
</ul>

<p>Bạn có thể thực hiện thao tác sau <strong>bất kỳ</strong> số lần nào:</p>

<p>Chọn một chỉ số <code>i</code> (với <code>0 &lt;= i &lt; n</code>) và đổi ký tự tại chỉ số đó thành một trong hai ký tự sau:</p>

<ul>
	<li>Ký tự ngay <strong>trước</strong> nó trong bảng chữ cái (nếu <code>caption[i] != &#39;a&#39;</code>).</li>
	<li>Ký tự ngay <strong>sau</strong> nó trong bảng chữ cái (nếu <code>caption[i] != &#39;z&#39;</code>).</li>
</ul>

<p>Nhiệm vụ của bạn là chuyển <code>caption</code> đã cho thành một caption <strong>hợp lệ</strong> bằng <strong>ít nhất</strong> số thao tác và trả về caption đó. Nếu có <strong>nhiều</strong> caption hợp lệ, hãy trả về caption có thứ tự <strong><span data-keyword="lexicographically-smaller-string">từ điển nhỏ nhất</span></strong> trong số đó. Nếu <strong>không thể</strong> tạo được caption, trả về một chuỗi rỗng <code>&quot;&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">caption = &quot;cdcd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;cccc&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể chứng minh rằng không thể chuyển caption đã cho thành một caption hợp lệ với ít hơn 2 thao tác. Các caption hợp lệ có thể tạo được bằng đúng 2 thao tác là:</p>

<ul>
	<li><code>&quot;dddd&quot;</code>: Đổi <code>caption[0]</code> và <code>caption[2]</code> thành ký tự tiếp theo là <code>&#39;d&#39;</code>.</li>
	<li><code>&quot;cccc&quot;</code>: Đổi <code>caption[1]</code> và <code>caption[3]</code> thành ký tự trước đó là <code>&#39;c&#39;</code>.</li>
</ul>

<p>Vì <code>&quot;cccc&quot;</code> có thứ tự từ điển nhỏ hơn <code>&quot;dddd&quot;</code>, nên trả về <code>&quot;cccc&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">caption = &quot;aca&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aaa&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể chứng minh rằng cần ít nhất 2 thao tác để chuyển caption đã cho thành một caption hợp lệ. Caption hợp lệ duy nhất có thể tạo được bằng đúng 2 thao tác là:</p>

<ul>
	<li>Thao tác 1: Đổi <code>caption[1]</code> thành <code>&#39;b&#39;</code>. <code>caption = &quot;aba&quot;</code>.</li>
	<li>Thao tác 2: Đổi <code>caption[1]</code> thành <code>&#39;a&#39;</code>. <code>caption = &quot;aaa&quot;</code>.</li>
</ul>

<p>Do đó, trả về <code>&quot;aaa&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">caption = &quot;bc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể chứng minh rằng không thể chuyển caption thành một caption hợp lệ bằng bất kỳ số thao tác nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= caption.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>caption</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một caption hợp lệ chia chuỗi thành các đoạn gồm các chữ cái giống nhau, mỗi đoạn có độ dài ít nhất $3$. Chi phí đổi một chữ cái là khoảng cách trong bảng chữ cái, và $n\le 5\times 10^4$.
>
> Một đoạn có thể dài hơn $3$, nhưng một đoạn quá dài có thể được tách ra. Tại vị trí $i$, quyết định cần đưa ra là chữ cái $c$ và độ dài $L\ge 3$ của đoạn tiếp theo.
>
> DP $f[i][c]$ là chi phí nhỏ nhất từ vị trí $i$ trở đi khi đoạn hiện tại có chữ cái $c$. Các chuyển trạng thái duyệt qua đoạn tiếp theo; các trạng thái trước đó được dùng để dựng lại chuỗi có thứ tự từ điển nhỏ nhất.

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
