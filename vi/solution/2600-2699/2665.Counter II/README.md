---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2665. Counter II](https://leetcode.com/problems/counter-ii)

[中文文档](/solution/2600-2699/2665.Counter%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết một hàm <code>createCounter</code>. Hàm này nhận vào một số nguyên ban đầu <code>init</code> và trả về một object gồm ba hàm.</p>

<p>Ba hàm đó là:</p>

<ul>
	<li><code>increment()</code>&nbsp;tăng giá trị hiện tại lên 1 rồi trả về giá trị đó.</li>
	<li><code>decrement()</code>&nbsp;giảm giá trị hiện tại đi 1 rồi trả về giá trị đó.</li>
	<li><code>reset()</code>&nbsp;đặt giá trị hiện tại về <code>init</code> rồi trả về giá trị đó.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> init = 5, calls = [&quot;increment&quot;,&quot;reset&quot;,&quot;decrement&quot;]
<strong>Đầu ra:</strong> [6,5,4]
<strong>Giải thích:</strong>
const counter = createCounter(5);
counter.increment(); // 6
counter.reset(); // 5
counter.decrement(); // 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> init = 0, calls = [&quot;increment&quot;,&quot;increment&quot;,&quot;decrement&quot;,&quot;reset&quot;,&quot;reset&quot;]
<strong>Đầu ra:</strong> [1,2,1,0,0]
<strong>Giải thích:</strong>
const counter = createCounter(0);
counter.increment(); // 1
counter.increment(); // 2
counter.decrement(); // 1
counter.reset(); // 0
counter.reset(); // 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-1000 &lt;= init &lt;= 1000</code></li>
	<li><code>0 &lt;= calls.length &lt;= 1000</code></li>
	<li><code>calls[i]</code> là một trong ba giá trị &quot;increment&quot;, &quot;decrement&quot; và&nbsp;&quot;reset&quot;</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Increment, decrement và reset phải dùng chung một counter. Nếu dùng các biến toàn cục riêng biệt, chúng sẽ xung đột; đóng gói $val$ trong closure giúp mỗi instance có state riêng.
>
> `increment`/`decrement` cập nhật $val$ rồi trả về giá trị đó; `reset` gán lại $init$ ban đầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type ReturnObj = {
    increment: () => number;
    decrement: () => number;
    reset: () => number;
};

function createCounter(init: number): ReturnObj {
    let val = init;
    return {
        increment() {
            return ++val;
        },
        decrement() {
            return --val;
        },
        reset() {
            return (val = init);
        },
    };
}

/**
 * const counter = createCounter(5)
 * counter.increment(); // 6
 * counter.reset(); // 5
 * counter.decrement(); // 4
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
