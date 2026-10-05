---
comments: true
difficulty: Hard
rating: 2375
source: Biweekly Contest 173 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3797. Count Routes to Climb a Rectangular Grid](https://leetcode.com/problems/count-routes-to-climb-a-rectangular-grid)

[中文文档](/solution/3700-3799/3797.Count%20Routes%20to%20Climb%20a%20Rectangular%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>grid</code> có kích thước <code>n</code>, trong đó mỗi chuỗi <code>grid[i]</code> có độ dài <code>m</code>. Ký tự <code>grid[i][j]</code> là một trong các ký hiệu sau:</p>

<ul>
	<li><code>&#39;.&#39;</code>: Ô này có thể đi qua.</li>
	<li><code>&#39;#&#39;</code>: Ô này bị chặn.</li>
</ul>

<p>Bạn cần đếm số route khác nhau để đi qua <code>grid</code>. Mỗi route phải bắt đầu từ <em>bất kỳ ô nào</em> ở hàng dưới cùng (hàng <code>n - 1</code>) và kết thúc ở hàng trên cùng (hàng 0).</p>

<p>Tuy nhiên, route có một số ràng buộc sau:</p>

<ul>
	<li>Chỉ được di chuyển từ một ô có thể đi qua đến <strong>một</strong> ô khác cũng có thể đi qua.</li>
	<li><strong>Khoảng cách Euclid</strong> của mỗi bước di chuyển <strong>không vượt quá</strong> <code>d</code>, trong đó <code>d</code> là một tham số nguyên được cho. Khoảng cách Euclid giữa hai ô <code>(r1, c1)</code>, <code>(r2, c2)</code> là <code>sqrt((r1 - r2)<sup>2</sup> + (c1 - c2)<sup>2</sup>)</code>.</li>
	<li>Mỗi bước di chuyển hoặc ở lại cùng hàng, hoặc đi lên hàng ngay phía trên (từ hàng <code>r</code> đến hàng <code>r - 1</code>).</li>
	<li>Không được ở lại cùng một hàng trong hai lượt liên tiếp. Nếu ở lại cùng hàng trong một bước di chuyển (và đây không phải bước cuối), bước tiếp theo phải đi lên hàng phía trên.</li>
</ul>

<p>Trả về một số nguyên biểu thị số route như vậy. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [&quot;..&quot;,&quot;#.&quot;], d = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta đánh số các ô đi qua trong route theo thứ tự, bắt đầu từ 1. Hai route là:</p>

<pre>
.2
#1
</pre>

<pre>
32
#1
</pre>

<p>Ta có thể di chuyển từ ô (1, 1) đến ô (0, 1) vì khoảng cách Euclid là <code>sqrt((1 - 0)<sup>2</sup> + (1 - 1)<sup>2</sup>) = sqrt(1) &lt;= d</code>.</p>

<p>Tuy nhiên, không thể di chuyển từ ô (1, 1) đến ô (0, 0) vì khoảng cách Euclid là <code>sqrt((1 - 0)<sup>2</sup> + (1 - 0)<sup>2</sup>) = sqrt(2) &gt; d</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [&quot;..&quot;,&quot;#.&quot;], d = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai route đã được nêu trong ví dụ 1. Hai route còn lại là:</p>

<pre>
2.
#1
</pre>

<pre>
23
#1
</pre>

<p>Lưu ý rằng ta có thể di chuyển từ (1, 1) đến (0, 0) vì khoảng cách Euclid là <code>sqrt(2) &lt;= d</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [&quot;#&quot;], d = 750</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta không thể chọn ô nào làm ô bắt đầu. Do đó, không có route nào.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [&quot;..&quot;], d = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các route có thể là:</p>

<pre>
.1
</pre>

<pre>
1.
</pre>

<pre>
12
</pre>

<pre>
21
</pre>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == grid.length &lt;= 750</code></li>
	<li><code>1 &lt;= m == grid[i].length &lt;= 750</code></li>
	<li><code>grid[i][j]</code> là <code>&#39;.&#39;</code> hoặc <code>&#39;#&#39;</code>.</li>
	<li><code>1 &lt;= d &lt;= 750</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các route bắt đầu ở hàng cuối và kết thúc ở hàng đầu, mỗi bước có độ dài Euclid không vượt quá $d$ và phải đi lên. Khi kích thước lưới vừa phải, ta tiền xử lý các ô lân cận hợp lệ ở hàng phía trên cho mọi ô trống, sau đó dùng DP từ hàng dưới cùng và tính tổng số cách theo modulo $10^9+7$.

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
