---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2797. Partial Function with Placeholders 🔒](https://leetcode.com/problems/partial-function-with-placeholders)

[中文文档](/solution/2700-2799/2797.Partial%20Function%20with%20Placeholders/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm <code>fn</code> và một mảng <code>args</code>, hãy trả về một hàm <code>partialFn</code>.</p>

<p>Các placeholder <code>&quot;_&quot;</code> trong <code>args</code> phải được thay thế bằng các giá trị từ <code>restArgs</code> bắt đầu từ chỉ số <code>0</code>. Mọi giá trị còn lại trong <code>restArgs</code> phải được thêm vào cuối <code>args</code>.</p>

<p><code>partialFn</code> phải trả về kết quả của <code>fn</code>. <code>fn</code> phải được gọi với các phần tử của <code>args</code> sau khi chỉnh sửa, được truyền dưới dạng các đối số riêng biệt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (...args) =&gt; args, args = [2,4,6], restArgs = [8,10]
<strong>Đầu ra:</strong> [2,4,6,8,10]
<strong>Giải thích:</strong>
const partialFn = partial(fn, args)
const result = partialFn(...restArgs)
console.log(result) //&nbsp;[2,4,6,8,10]

Không có placeholder &quot;_&quot; nào trong args, vì vậy restArgs chỉ được thêm vào cuối args. Sau đó, các phần tử của args được truyền cho fn dưới dạng các đối số riêng biệt, và fn trả về các đối số đã truyền dưới dạng một mảng.
</pre>

<strong class="example">Ví dụ 2:</strong>

<pre>
<strong>Đầu vào:</strong> fn = (...args) =&gt; args, args = [1,2,&quot;_&quot;,4,&quot;_&quot;,6], restArgs = [3,5]
<strong>Đầu ra:</strong> [1,2,3,4,5,6]
<strong>Giải thích:</strong>
const partialFn = partial(fn, args)
const result = partialFn(...restArgs)
console.log(result) //&nbsp;[1,2,3,4,5,6]

Các placeholder &quot;_&quot; được thay thế bằng các giá trị từ restArgs. Sau đó, các phần tử của args được truyền cho fn dưới dạng các đối số riêng biệt, và fn trả về các đối số đã truyền dưới dạng một mảng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (a, b, c) =&gt; b + a - c, args = [&quot;_&quot;, 5], restArgs = [5, 20]
<strong>Đầu ra:</strong> -10
<strong>Giải thích:</strong>
const partialFn = partial(fn, args)
const result = partialFn(...restArgs)
console.log(result) //&nbsp;-10

Placeholder &quot;_&quot; được thay thế bằng 5 và 20 được thêm vào cuối args. Sau đó, các phần tử của args được truyền cho fn dưới dạng các đối số riêng biệt, và fn trả về -10 (5 + 5 - 20).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>fn</code> là một hàm</li>
	<li><code>args</code> và <code>restArgs</code> là các mảng JSON hợp lệ</li>
	<li><code>1 &lt;= args.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;=&nbsp;restArgs.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= number of placeholders &lt;= restArgs.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Partial application cần điền các placeholder $\_$ bằng các đối số truyền vào sau theo đúng thứ tự, rồi thêm phần còn lại vào cuối. $bind$ thông thường không thể biểu diễn placeholder.
>
> Hàm được trả về sẽ duyệt qua mảng đối số ban đầu, thay mỗi $\_$ bằng đối số tiếp theo, $push$ mọi đối số còn dư, rồi $apply$ hàm ban đầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function partial(fn: Function, args: any[]): Function {
    return function (...restArgs) {
        let i = 0;
        for (let j = 0; j < args.length; ++j) {
            if (args[j] === '_') {
                args[j] = restArgs[i++];
            }
        }
        while (i < restArgs.length) {
            args.push(restArgs[i++]);
        }
        return fn(...args);
    };
}
```

#### JavaScript

```js
/**
 * @param {Function} fn
 * @param {Array} args
 * @return {Function}
 */
var partial = function (fn, args) {
    return function (...restArgs) {
        let i = 0;
        for (let j = 0; j < args.length; ++j) {
            if (args[j] === '_') {
                args[j] = restArgs[i++];
            }
        }
        while (i < restArgs.length) {
            args.push(restArgs[i++]);
        }
        return fn.apply(this, args);
    };
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
