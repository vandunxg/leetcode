---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2666. Allow One Function Call](https://leetcode.com/problems/allow-one-function-call)

[中文文档](/solution/2600-2699/2666.Allow%20One%20Function%20Call/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm <code>fn</code>, hãy trả về một hàm mới giống hệt hàm ban đầu, ngoại trừ việc đảm bảo&nbsp;<code>fn</code>&nbsp;được gọi nhiều nhất một lần.</p>

<ul>
	<li>Lần đầu tiên hàm được trả về được gọi, nó phải trả về cùng kết quả như&nbsp;<code>fn</code>.</li>
	<li>Mọi lần gọi sau đó, nó phải trả về&nbsp;<code>undefined</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (a,b,c) =&gt; (a + b + c), calls = [[1,2,3],[2,3,6]]
<strong>Đầu ra:</strong> [{&quot;calls&quot;:1,&quot;value&quot;:6}]
<strong>Giải thích:</strong>
const onceFn = once(fn);
onceFn(1, 2, 3); // 6
onceFn(2, 3, 6); // undefined, fn was not called
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (a,b,c) =&gt; (a * b * c), calls = [[5,7,4],[2,3,6],[4,6,8]]
<strong>Đầu ra:</strong> [{&quot;calls&quot;:1,&quot;value&quot;:140}]
<strong>Giải thích:</strong>
const onceFn = once(fn);
onceFn(5, 7, 4); // 140
onceFn(2, 3, 6); // undefined, fn was not called
onceFn(4, 6, 8); // undefined, fn was not called
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>calls</code> là một mảng JSON hợp lệ</li>
	<li><code>1 &lt;= calls.length &lt;= 10</code></li>
	<li><code>1 &lt;= calls[i].length &lt;= 100</code></li>
	<li><code>2 &lt;= JSON.stringify(calls).length &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hàm wrapper phải chạy hàm ban đầu chỉ một lần. Nếu không có cờ, mỗi lần gọi sẽ lại đi vào hàm đó.
>
> Biến $called$ được closure capture sẽ chuyển trạng thái sau lần gọi đầu tiên; các lần gọi sau trả về `undefined`.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type JSONValue = null | boolean | number | string | JSONValue[] | { [key: string]: JSONValue };
type OnceFn = (...args: JSONValue[]) => JSONValue | undefined;

function once(fn: Function): OnceFn {
    let called = false;
    return function (...args) {
        if (!called) {
            called = true;
            return fn(...args);
        }
    };
}

/**
 * let fn = (a,b,c) => (a + b + c)
 * let onceFn = once(fn)
 *
 * onceFn(1,2,3); // 6
 * onceFn(2,3,6); // returns undefined without calling fn
 */
```

#### JavaScript

```js
/**
 * @param {Function} fn
 * @return {Function}
 */
var once = function (fn) {
    let called = false;
    return function (...args) {
        if (!called) {
            called = true;
            return fn(...args);
        }
    };
};

/**
 * let fn = (a,b,c) => (a + b + c)
 * let onceFn = once(fn)
 *
 * onceFn(1,2,3); // 6
 * onceFn(2,3,6); // returns undefined without calling fn
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
