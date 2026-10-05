---
comments: true
difficulty: Hard
rating: 2100
source: Biweekly Contest 187 Q4
tags:
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3995. Minimum Cost to Convert String III](https://leetcode.com/problems/minimum-cost-to-convert-string-iii)

[中文文档](/solution/3900-3999/3995.Minimum%20Cost%20to%20Convert%20String%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>source</code> và <code>target</code>.</p>

<p>Ta cũng có một mảng chuỗi 2 chiều <code>rules</code>, trong đó <code>rules[i] = [pattern<sub>i</sub>, replacement<sub>i</sub>]</code>, và một mảng số nguyên <code>costs</code>, trong đó <code>costs[i]</code> là chi phí cơ bản khi áp dụng <code>rules[i]</code>. Hai mảng có cùng độ dài. Ngoài ra, <code>pattern<sub>i</sub></code> và <code>replacement<sub>i</sub></code> có cùng độ dài.</p>

<p>Ta có thể áp dụng <strong>bất kỳ</strong> rule nào <strong>bao nhiêu lần tùy ý</strong>. Mỗi lần áp dụng một rule được thực hiện như sau:</p>

<ul>
	<li>Chọn một chỉ số <code>l</code> sao cho đoạn vị trí từ <code>l</code> đến <code>l + pattern<sub>i</sub>.length - 1</code> tồn tại trong chuỗi hiện tại và <strong>không vị trí nào trong đoạn này đã được sử dụng trong lần áp dụng rule trước đó</strong>.</li>
	<li>Với mỗi chỉ số <code>j</code>, ký tự <code>pattern<sub>i</sub>[j]</code> phải <strong>bằng</strong> ký tự hiện tại ở vị trí <code>l + j</code>, hoặc là <code>&#39;*&#39;</code>.</li>
	<li>Thay các ký tự trong đoạn này bằng <code>replacement<sub>i</sub></code>. Chuỗi thay thế được dùng <strong>đúng nguyên dạng</strong> và không chứa wildcard.</li>
	<li>Chi phí của lần áp dụng rule này là <code>costs[i]</code> <strong>cộng với</strong> số ký tự <code>&#39;*&#39;</code> trong <code>pattern<sub>i</sub></code>.</li>
	<li>Một khi một vị trí ký tự đã được sử dụng trong một lần áp dụng rule, vị trí đó <strong>không thể</strong> được sử dụng trong bất kỳ lần áp dụng rule <strong>sau đó</strong>.</li>
</ul>

<p>Vì mọi <code>pattern<sub>i</sub></code> và <code>replacement<sub>i</sub></code> có cùng độ dài, các vị trí ký tự được giữ nguyên sau mỗi lần áp dụng rule.</p>

<p>Trả về <strong>chi phí tổng nhỏ nhất</strong> cần thiết để biến đổi <code>source</code> thành <code>target</code>. Nếu không thể thực hiện, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = &quot;hello&quot;, target = &quot;world&quot;, rules = [[&quot;he&quot;,&quot;wo&quot;],[&quot;llo&quot;,&quot;rld&quot;]], costs = [3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Áp dụng <code>rules[0]</code> để thay <code>&quot;he&quot;</code> bằng <code>&quot;wo&quot;</code> với chi phí 3, khi đó chuỗi trở thành <code>&quot;wollo&quot;</code>.</li>
	<li>Áp dụng <code>rules[1]</code> để thay <code>&quot;llo&quot;</code> bằng <code>&quot;rld&quot;</code> với chi phí 4, khi đó chuỗi trở thành <code>&quot;world&quot;</code>.</li>
	<li>Chi phí tổng là <code>3 + 4 = 7</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = &quot;cat&quot;, target = &quot;dog&quot;, rules = [[&quot;c*t&quot;,&quot;dog&quot;]], costs = [2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Áp dụng <code>rules[0]</code> để thay <code>&quot;cat&quot;</code> bằng <code>&quot;dog&quot;</code>. Wildcard <code>&#39;*&#39;</code> khớp với <code>&#39;a&#39;</code>, cộng thêm 1 vào chi phí cơ bản 2.</li>
	<li>Chi phí tổng là <code>2 + 1 = 3</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = &quot;test&quot;, target = &quot;next&quot;, rules = [[&quot;*e*t&quot;,&quot;next&quot;]], costs = [4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Áp dụng <code>rules[0]</code> để thay <code>&quot;test&quot;</code> bằng <code>&quot;next&quot;</code>. Wildcard đầu tiên khớp với <code>&#39;t&#39;</code> và wildcard thứ hai khớp với <code>&#39;s&#39;</code>, cộng thêm 2 vào chi phí cơ bản 4.</li>
	<li>Chi phí tổng là <code>4 + 2 = 6</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = &quot;ab&quot;, target = &quot;bc&quot;, rules = [[&quot;a*&quot;,&quot;bd&quot;]], costs = [9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có chuỗi lần áp dụng rule nào có thể biến đổi <code>source</code> thành <code>target</code>, nên đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= source.length == target.length &lt;= 5000</code></li>
	<li><code>source</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= rules.length == costs.length &lt;= 200</code></li>
	<li><code>rules[i] = [pattern<sub>i</sub>, replacement<sub>i</sub>]</code></li>
	<li><code>1 &lt;= pattern<sub>i</sub>.length == replacement<sub>i</sub>.length &lt;= 20</code></li>
	<li><code>pattern<sub>i</sub></code> chứa ít nhất một chữ cái tiếng Anh viết thường và nhiều nhất 5 ký tự <code>&#39;*&#39;</code>.</li>
	<li><code>replacement<sub>i</sub></code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= costs[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi rule bao phủ một đoạn và các vị trí đã dùng không thể được dùng lại, vì vậy $\textit{source}$ được chia thành các đoạn khớp rời nhau, mỗi đoạn được viết lại thành đoạn tương ứng của $\textit{target}$. Mỗi `*` cộng thêm một đơn vị vào chi phí.
>
> Từ mỗi chỉ số, ta tiền xử lý các rule có thể áp dụng rồi tìm đường đi ngắn nhất trên các chỉ số: một cạnh $i\to i+|\textit{pattern}|$ có trọng số bằng chi phí của rule cộng với số dấu sao, và chỉ tồn tại khi pattern khớp và replacement bằng đoạn tương ứng của target.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng lại ở bài toán tìm đường đi ngắn nhất trên các chỉ số.

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
