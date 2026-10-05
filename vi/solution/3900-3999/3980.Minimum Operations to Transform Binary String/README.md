---
comments: true
difficulty: Medium
rating: 1845
source: Biweekly Contest 186 Q3
tags:
    - Greedy
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3980. Minimum Operations to Transform Binary String](https://leetcode.com/problems/minimum-operations-to-transform-binary-string)

[中文文档](/solution/3900-3999/3980.Minimum%20Operations%20to%20Transform%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai <span data-keyword="binary-string">chuỗi nhị phân</span> <code>s1</code> và <code>s2</code> có cùng độ dài <code>n</code>.</p>

<p>Bạn có thể thực hiện các thao tác sau trên <code>s1</code> một số lần bất kỳ, theo thứ tự bất kỳ:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> sao cho <code>s1[i] == &#39;0&#39;</code>, rồi đổi nó thành <code>&#39;1&#39;</code>.</li>
	<li>Chọn một chỉ số <code>i</code> sao cho <code>0 &lt;= i &lt; n - 1</code>, đồng thời <code>s1[i]</code> và <code>s1[i + 1]</code> đều là <code>&#39;1&#39;</code>. Đổi cả hai ký tự thành <code>&#39;0&#39;</code>.</li>
</ul>

<p>Trả về số thao tác <strong>ít nhất</strong> cần thực hiện để biến <code>s1</code> thành <code>s2</code>. Nếu không thể thực hiện được, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s1 = &quot;11&quot;, s2 = &quot;00&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đổi các chỉ số 0 và 1 từ <code>&#39;1&#39;</code> thành <code>&#39;0&#39;</code> trong một thao tác, khi đó <code>&quot;11&quot;</code> trở thành <code>&quot;00&quot;</code>. Vì vậy, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s1 = &quot;01&quot;, s2 = &quot;10&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đổi chỉ số 0 từ <code>&#39;0&#39;</code> thành <code>&#39;1&#39;</code>, khi đó <code>&quot;01&quot;</code> trở thành <code>&quot;11&quot;</code>.</li>
	<li>Đổi các chỉ số 0 và 1 từ <code>&#39;1&#39;</code> thành <code>&#39;0&#39;</code>, khi đó <code>&quot;11&quot;</code> trở thành <code>&quot;00&quot;</code>.</li>
	<li>Đổi chỉ số 0 từ <code>&#39;0&#39;</code> thành <code>&#39;1&#39;</code>, khi đó <code>&quot;00&quot;</code> trở thành <code>&quot;10&quot;</code>.</li>
	<li>Vì vậy, đáp án là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s1 = &quot;1&quot;, s2 = &quot;0&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thao tác đầu tiên không thể đổi <code>&#39;1&#39;</code> thành <code>&#39;0&#39;</code>, còn thao tác thứ hai yêu cầu hai ký tự kề nhau. Do đó, không thể thực hiện được.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s1.length == s2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thao tác thứ nhất đổi một $0$ đơn lẻ thành $1$; thao tác thứ hai đổi `11` kề nhau thành `00`. Với $n\le 10^5$, không thể tìm kiếm các chuỗi thao tác.
>
> Ta tham lam từ trái sang phải: bỏ qua bit đã khớp; nếu gặp $0$ nhưng cần là $1$ thì dùng thao tác thứ nhất; nếu gặp $1$ nhưng cần là $0$ thì phải ghép với bit tiếp theo thành `11` rồi đổi cả hai, nếu không thì không thể thực hiện.
>
> Thư mục này hiện chưa có lời giải được cài đặt; phần trình bày dừng ở chiến lược tham lam theo từng bit.

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
