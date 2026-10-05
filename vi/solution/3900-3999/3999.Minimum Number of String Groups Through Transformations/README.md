---
comments: true
difficulty: Hard
rating: 2161
source: Weekly Contest 511 Q4
tags:
    - Hash Table
    - Two Pointers
    - String
---

<!-- problem:start -->

# [3999. Minimum Number of String Groups Through Transformations](https://leetcode.com/problems/minimum-number-of-string-groups-through-transformations)

[中文文档](/solution/3900-3999/3999.Minimum%20Number%20of%20String%20Groups%20Through%20Transformations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code>.</p>

<p>Định nghĩa một <strong>phép biến đổi</strong> trên chuỗi <code>s</code> như sau:</p>

<ul>
	<li>Gọi <code>E</code> là <span data-keyword="subsequence-string">dãy con</span> gồm các ký tự ở chỉ số chẵn của <code>s</code>.</li>
	<li>Gọi <code>O</code> là <strong>dãy con</strong> gồm các ký tự ở chỉ số lẻ của <code>s</code>.</li>
	<li><strong>Độc lập</strong> dịch vòng <code>E</code> và <code>O</code> sang phải một số vị trí <strong>bất kỳ</strong>, có thể bằng không.</li>
	<li>Dựng lại chuỗi bằng cách đặt các ký tự của <code>E</code> đã dịch trở lại vào các chỉ số chẵn và các ký tự của <code>O</code> đã dịch vào các chỉ số lẻ.</li>
</ul>

<p>Hai chuỗi được gọi là <strong>tương đương</strong> nếu một chuỗi có thể được biến đổi thành chuỗi kia bằng <strong>một phép biến đổi duy nhất</strong>.</p>

<p>Chia <code>words</code> thành <strong>số nhóm nhỏ nhất</strong> sao cho:</p>

<ul>
	<li>Mỗi chuỗi thuộc về <strong>đúng</strong> một nhóm.</li>
	<li>Mọi cặp chuỗi trong cùng một nhóm đều <strong>tương đương</strong>.</li>
</ul>

<p>Trả về một số nguyên biểu thị <strong>số nhóm nhỏ nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;ntgwz&quot;,&quot;zwntg&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>&quot;ntgwz&quot;</code>, dãy con tại các chỉ số chẵn là <code>&quot;ngz&quot;</code> và dãy con tại các chỉ số lẻ là <code>&quot;tw&quot;</code>.</li>
	<li>Dịch <code>&quot;ngz&quot;</code> sang phải <code>1</code> vị trí để được <code>&quot;zng&quot;</code>, và dịch <code>&quot;tw&quot;</code> sang phải <code>1</code> vị trí để được <code>&quot;wt&quot;</code>.</li>
	<li>Sau khi dựng lại chuỗi, ta được <code>&quot;zwntg&quot;</code>.</li>
	<li>Vì vậy, hai chuỗi tương đương và thuộc cùng một nhóm.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abc&quot;,&quot;cab&quot;,&quot;bac&quot;,&quot;acb&quot;,&quot;bca&quot;,&quot;cba&quot;]</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi có thể được chia thành các nhóm sau:</p>

<ul>
	<li><code>[&quot;abc&quot;,&quot;cba&quot;]</code></li>
	<li><code>[&quot;cab&quot;,&quot;bac&quot;]</code></li>
	<li><code>[&quot;acb&quot;,&quot;bca&quot;]</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;leet&quot;,&quot;abb&quot;,&quot;bab&quot;,&quot;deed&quot;,&quot;edde&quot;,&quot;code&quot;,&quot;bba&quot;]</span></p>

<p><strong>Đầu ra:</strong> 5</p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi có thể được chia thành các nhóm sau:</p>

<ul>
	<li><code>[&quot;abb&quot;,&quot;bba&quot;]</code></li>
	<li><code>[&quot;deed&quot;,&quot;edde&quot;]</code></li>
	<li><code>[&quot;leet&quot;]</code></li>
	<li><code>[&quot;bab&quot;]</code></li>
	<li><code>[&quot;code&quot;]</code></li>
</ul>

<p>​​​​​​​​​​​​​​Mọi cặp chuỗi trong mỗi nhóm đều tương đương.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 5 * 10<sup>5</sup></code></li>
	<li>Tổng <code>words[i].length</code> không vượt quá <code>5 * 10<sup>5</sup></code>.</li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một phép biến đổi xoay vòng dãy con ở các chỉ số chẵn và dãy con ở các chỉ số lẻ một cách độc lập. Hai chuỗi tương đương khi và chỉ khi hai dãy con này có cùng các phần tử — phép xoay bảo toàn các phần tử, và một phép xoay có thể tạo ra mọi dịch chuyển vòng.
>
> Nếu chỉ cần so sánh hai chuỗi chẵn và lẻ đã sắp xếp, mỗi lớp tương đương được biểu diễn bởi một cặp tuple đã sắp xếp, và số nhóm là số cặp phân biệt. Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng lại ở bất biến của phép xoay chẵn/lẻ đó.

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
