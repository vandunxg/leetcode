---
comments: true
difficulty: Hard
rating: 2286
source: Weekly Contest 438 Q3
tags:
    - Math
    - String
    - Combinatorics
    - Number Theory
---

<!-- problem:start -->

# [3463. Check If Digits Are Equal in String After Operations II](https://leetcode.com/problems/check-if-digits-are-equal-in-string-after-operations-ii)

[中文文档](/solution/3400-3499/3463.Check%20If%20Digits%20Are%20Equal%20in%20String%20After%20Operations%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ số. Lặp lại thao tác sau cho đến khi chuỗi có <strong>chính xác</strong> hai chữ số:</p>

<ul>
	<li>Với mỗi cặp chữ số liên tiếp trong <code>s</code>, bắt đầu từ chữ số đầu tiên, tính một chữ số mới bằng tổng của hai chữ số đó <strong>theo modulo</strong> 10.</li>
	<li>Thay <code>s</code> bằng dãy các chữ số mới được tính, <em>giữ nguyên thứ tự</em> mà chúng được tính.</li>
</ul>

<p>Trả về <code>true</code> nếu hai chữ số cuối cùng trong <code>s</code> <strong>giống nhau</strong>; nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;3902&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu, <code>s = &quot;3902&quot;</code></li>
	<li>Thao tác đầu tiên:
	<ul>
		<li><code>(s[0] + s[1]) % 10 = (3 + 9) % 10 = 2</code></li>
		<li><code>(s[1] + s[2]) % 10 = (9 + 0) % 10 = 9</code></li>
		<li><code>(s[2] + s[3]) % 10 = (0 + 2) % 10 = 2</code></li>
		<li><code>s</code> trở thành <code>&quot;292&quot;</code></li>
	</ul>
	</li>
	<li>Thao tác thứ hai:
	<ul>
		<li><code>(s[0] + s[1]) % 10 = (2 + 9) % 10 = 1</code></li>
		<li><code>(s[1] + s[2]) % 10 = (9 + 2) % 10 = 1</code></li>
		<li><code>s</code> trở thành <code>&quot;11&quot;</code></li>
	</ul>
	</li>
	<li>Vì các chữ số trong <code>&quot;11&quot;</code> giống nhau nên kết quả là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;34789&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu, <code>s = &quot;34789&quot;</code>.</li>
	<li>Sau thao tác đầu tiên, <code>s = &quot;7157&quot;</code>.</li>
	<li>Sau thao tác thứ hai, <code>s = &quot;862&quot;</code>.</li>
	<li>Sau thao tác thứ ba, <code>s = &quot;48&quot;</code>.</li>
	<li>Vì <code>&#39;4&#39; != &#39;8&#39;</code> nên kết quả là <code>false</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Quy trình này giống phần I, nhưng $n\le 10^5$ khiến việc mô phỏng theo từng lớp là không khả thi. Hai chữ số cuối là các dạng tuyến tính của chuỗi ban đầu với các hệ số nhị thức modulo $10$.
>
> Chỉ số $i$ đóng góp $C_{n-2}^{i}\,s[i]$ (hoặc $C_{n-2}^{i-1}$) vào chữ số bên trái (bên phải). Vì $10$ là hợp số, ta áp dụng Lucas modulo $2$ và $5$, rồi kết hợp bằng CRT.
>
> So sánh hai tổng có trọng số theo modulo $10$.

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
