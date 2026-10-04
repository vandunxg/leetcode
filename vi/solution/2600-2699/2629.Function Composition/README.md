---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2629. Function Composition](https://leetcode.com/problems/function-composition)

[中文文档](/solution/2600-2699/2629.Function%20Composition/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các hàm&nbsp;<code>[f<span style="font-size: 10.8333px;">1</span>, f<sub>2</sub>, f<sub>3</sub>,&nbsp;..., f<sub>n</sub>]</code>, hãy trả về một hàm mới&nbsp;<code>fn</code>&nbsp;là <strong>phép hợp thành của các hàm</strong> trong mảng.</p>

<p><strong>Phép hợp thành của các hàm</strong>&nbsp;<code>[f(x), g(x), h(x)]</code>&nbsp;là&nbsp;<code>fn(x) = f(g(h(x)))</code>.</p>

<p><strong>Phép hợp thành của các hàm</strong>&nbsp;trong một danh sách rỗng là <strong>hàm đồng nhất</strong>&nbsp;<code>f(x) = x</code>.</p>

<p>Có thể giả sử mỗi&nbsp;hàm trong mảng nhận một số nguyên làm đầu vào&nbsp;và trả về một số nguyên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [x =&gt; x + 1, x =&gt; x * x, x =&gt; 2 * x], x = 4
<strong>Đầu ra:</strong> 65
<strong>Giải thích:</strong>
Tính từ phải sang trái ...
Bắt đầu với x = 4.
2 * (4) = 8
(8) * (8) = 64
(64) + 1 = 65
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [x =&gt; 10 * x, x =&gt; 10 * x, x =&gt; 10 * x], x = 1
<strong>Đầu ra:</strong> 1000
<strong>Giải thích:</strong>
Tính từ phải sang trái ...
10 * (1) = 10
10 * (10) = 100
10 * (100) = 1000
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [], x = 42
<strong>Đầu ra:</strong> 42
<strong>Giải thích:</strong>
Phép hợp thành của không hàm nào là hàm đồng nhất</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code><font face="monospace">-1000 &lt;= x &lt;= 1000</font></code></li>
	<li><code><font face="monospace">0 &lt;= functions.length &lt;= 1000</font></code></li>
	<li>mọi hàm nhận vào và trả về một số nguyên</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phép hợp thành được áp dụng từ phải sang trái. Nếu dùng fold từ trái sang phải, thứ tự toán học sẽ bị đảo ngược. Chỉ cần duyệt một lần qua mảng ngắn này.
>
> `reduceRight` bắt đầu từ $x$ và áp dụng từng hàm; với danh sách rỗng, nó cho kết quả là hàm đồng nhất như yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type F = (x: number) => number;

function compose(functions: F[]): F {
    return function (x) {
        return functions.reduceRight((acc, fn) => fn(acc), x);
    };
}

/**
 * const fn = compose([x => x + 1, x => 2 * x])
 * fn(4) // 9
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
