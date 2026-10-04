---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2623. Memoize](https://leetcode.com/problems/memoize)

[中文文档](/solution/2600-2699/2623.Memoize/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm <code>fn</code>, hãy trả về phiên bản <strong>memoized</strong> của hàm đó.</p>

<p>Hàm <strong>memoized</strong> là hàm sẽ không bao giờ được gọi hai lần với cùng một đầu vào. Thay vào đó, hàm sẽ trả về giá trị đã được lưu trong cache.</p>

<p>Có thể giả định có <strong>3</strong> hàm đầu vào: <code>sum</code><strong>, </strong><code>fib</code><strong>,&nbsp;</strong>và&nbsp;<code>factorial</code><strong>.</strong></p>

<ul>
	<li><code>sum</code><strong>&nbsp;</strong>nhận hai số nguyên <code>a</code> và <code>b</code>, rồi trả về <code>a + b</code>. Giả sử nếu giá trị cho các đối số <code>(b, a)</code>, với <code>a != b</code>, đã được lưu trong cache thì không thể dùng giá trị đó cho các đối số <code>(a, b)</code>. Ví dụ, nếu các đối số là <code>(3, 2)</code> và <code>(2, 3)</code>, cần thực hiện hai lần gọi riêng biệt.</li>
	<li><code>fib</code><strong>&nbsp;</strong>nhận một số nguyên duy nhất <code>n</code> và trả về <code>1</code> nếu <font face="monospace"><code>n &lt;= 1</code> </font>hoặc<font face="monospace">&nbsp;<code>fib(n - 1) + fib(n - 2)</code>&nbsp;</font>trong các trường hợp còn lại.</li>
	<li><code>factorial</code>&nbsp;nhận một số nguyên duy nhất <code>n</code> và trả về <code>1</code>&nbsp;nếu&nbsp;<code>n &lt;= 1</code>&nbsp;hoặc&nbsp;<code>factorial(n - 1) * n</code>&nbsp;trong các trường hợp còn lại.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fnName = &quot;sum&quot;
actions = [&quot;call&quot;,&quot;call&quot;,&quot;getCallCount&quot;,&quot;call&quot;,&quot;getCallCount&quot;]
values = [[2,2],[2,2],[],[1,2],[]]
<strong>Đầu ra:</strong> [4,4,1,3,2]
<strong>Giải thích:</strong>
const sum = (a, b) =&gt; a + b;
const memoizedSum = memoize(sum);
memoizedSum(2, 2); // &quot;call&quot; - returns 4. sum() was called as (2, 2) was not seen before.
memoizedSum(2, 2); // &quot;call&quot; - returns 4. However sum() was not called because the same inputs were seen before.
// &quot;getCallCount&quot; - total call count: 1
memoizedSum(1, 2); // &quot;call&quot; - returns 3. sum() was called as (1, 2) was not seen before.
// &quot;getCallCount&quot; - total call count: 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:
</strong>fnName = &quot;factorial&quot;
actions = [&quot;call&quot;,&quot;call&quot;,&quot;call&quot;,&quot;getCallCount&quot;,&quot;call&quot;,&quot;getCallCount&quot;]
values = [[2],[3],[2],[],[3],[]]
<strong>Đầu ra:</strong> [2,6,2,2,6,2]
<strong>Giải thích:</strong>
const factorial = (n) =&gt; (n &lt;= 1) ? 1 : (n * factorial(n - 1));
const memoFactorial = memoize(factorial);
memoFactorial(2); // &quot;call&quot; - returns 2.
memoFactorial(3); // &quot;call&quot; - returns 6.
memoFactorial(2); // &quot;call&quot; - returns 2. However factorial was not called because 2 was seen before.
// &quot;getCallCount&quot; - total call count: 2
memoFactorial(3); // &quot;call&quot; - returns 6. However factorial was not called because 3 was seen before.
// &quot;getCallCount&quot; - total call count: 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:
</strong>fnName = &quot;fib&quot;
actions = [&quot;call&quot;,&quot;getCallCount&quot;]
values = [[5],[]]
<strong>Đầu ra:</strong> [8,1]
<strong>Giải thích:
</strong>fib(5) = 8 // &quot;call&quot;
// &quot;getCallCount&quot; - total call count: 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= a, b &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>1 &lt;= actions.length &lt;= 10<sup>5</sup></code></li>
	<li><code>actions.length === values.length</code></li>
	<li><code>actions[i]</code> là một trong hai giá trị &quot;call&quot; và &quot;getCallCount&quot;</li>
	<li><code>fnName</code> là một trong ba giá trị &quot;sum&quot;, &quot;factorial&quot; và&nbsp;&quot;fib&quot;</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các đối số giống nhau nên dùng lại kết quả trước đó. Nếu lần nào cũng gọi hàm gốc thì số lần gọi sẽ bị tăng không cần thiết. Các bộ giá trị nguyên thủy có thể dùng làm key của object.
>
> Một map dùng danh sách đối số làm key sẽ trả về kết quả ngay khi tìm thấy và lưu kết quả sau khi tính toán nếu chưa có.
>
> `args in cache` chuyển mảng thành chuỗi, đủ dùng với các đối số kiểu số trong bài toán này.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type Fn = (...params: any) => any;

function memoize(fn: Fn): Fn {
    const cache: Record<any, any> = {};

    return function (...args) {
        if (args in cache) {
            return cache[args];
        }
        const result = fn(...args);
        cache[args] = result;
        return result;
    };
}

/**
 * let callCount = 0;
 * const memoizedFn = memoize(function (a, b) {
 *	 callCount += 1;
 *   return a + b;
 * })
 * memoizedFn(2, 3) // 5
 * memoizedFn(2, 3) // 5
 * console.log(callCount) // 1
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
