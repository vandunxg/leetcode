---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2803. Factorial Generator 🔒](https://leetcode.com/problems/factorial-generator)

[中文文档](/solution/2800-2899/2803.Factorial%20Generator/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm generator nhận số nguyên <code>n</code> làm đối số và trả về một generator object tạo ra <strong>dãy giai thừa</strong>.</p>

<p><strong>Dãy giai thừa</strong> được định nghĩa bởi công thức <code>n!&nbsp;= n *&nbsp;<span style="font-size: 13px;">(</span>n-1)&nbsp;* (n-2)&nbsp;*&nbsp;...&nbsp;* 2 * 1​​​.</code></p>

<p>Giai thừa của 0 được định nghĩa là 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> [1,2,6,24,120]
<strong>Giải thích:</strong>
const gen = factorial(5)
gen.next().value // 1
gen.next().value // 2
gen.next().value // 6
gen.next().value // 24
gen.next().value // 120
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong>
const gen = factorial(2)
gen.next().value // 1
gen.next().value // 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 0
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong>
const gen = factorial(0)
gen.next().value // 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 18</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Generator cần tạo ra $0!$ hoặc $1!$ đến $n!$ theo nhu cầu; không cần tính trước toàn bộ các giai thừa. Ta duy trì một tích lũy kế, nhân với thừa số tiếp theo rồi tạo ra kết quả. Khi $n=0$, ta tạo ra $0!=1$ một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function* factorial(n: number): Generator<number> {
    if (n === 0) {
        yield 1;
    }
    let ans = 1;
    for (let i = 1; i <= n; ++i) {
        ans *= i;
        yield ans;
    }
}

/**
 * const gen = factorial(2);
 * gen.next().value; // 1
 * gen.next().value; // 2
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
