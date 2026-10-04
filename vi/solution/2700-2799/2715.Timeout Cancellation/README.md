---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2715. Timeout Cancellation](https://leetcode.com/problems/timeout-cancellation)

[中文文档](/solution/2700-2799/2715.Timeout%20Cancellation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm <code>fn</code>, một mảng&nbsp;đối số&nbsp;<code>args</code> và thời gian chờ&nbsp;<code>t</code>&nbsp;tính bằng mili giây, hãy trả về một hàm hủy <code>cancelFn</code>.</p>

<p>Sau khoảng trễ <code>cancelTimeMs</code>, hàm hủy <code>cancelFn</code> được trả về sẽ được gọi.</p>

<pre>
setTimeout(cancelFn, cancelTimeMs)
</pre>

<p>Ban đầu, việc thực thi hàm <code>fn</code> phải được trì hoãn <code>t</code> mili giây.</p>

<p>Nếu hàm <code>cancelFn</code> được gọi trước khi hết <code>t</code> mili giây, hàm này phải hủy việc thực thi đang bị trì hoãn của <code>fn</code>. Ngược lại, nếu <code>cancelFn</code> không được gọi trong khoảng trễ <code>t</code> đã chỉ định, <code>fn</code> phải được thực thi với <code>args</code> đã cho làm các đối số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (x) =&gt; x * 5, args = [2], t = 20
<strong>Đầu ra:</strong> [{&quot;time&quot;: 20, &quot;returned&quot;: 10}]
<strong>Giải thích:</strong>
const cancelTimeMs = 50;
const cancelFn = cancellable((x) =&gt; x * 5, [2], 20);
setTimeout(cancelFn, cancelTimeMs);

Lệnh hủy được lên lịch sau khoảng trễ cancelTimeMs (50ms), xảy ra sau khi fn(2) được thực thi ở thời điểm 20ms.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (x) =&gt; x**2, args = [2], t = 100
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong>
const cancelTimeMs = 50;
const cancelFn = cancellable((x) =&gt; x**2, [2], 100);
setTimeout(cancelFn, cancelTimeMs);

Lệnh hủy được lên lịch sau khoảng trễ cancelTimeMs (50ms), xảy ra trước khi fn(2) được thực thi ở thời điểm 100ms, nên fn(2) không bao giờ được gọi.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (x1, x2) =&gt; x1 * x2, args = [2,4], t = 30
<strong>Đầu ra:</strong> [{&quot;time&quot;: 30, &quot;returned&quot;: 8}]
<strong>Giải thích:
</strong>const cancelTimeMs = 100;
const cancelFn = cancellable((x1, x2) =&gt; x1 * x2, [2,4], 30);
setTimeout(cancelFn, cancelTimeMs);

Lệnh hủy được lên lịch sau khoảng trễ cancelTimeMs (100ms), xảy ra sau khi fn(2,4) được thực thi ở thời điểm 30ms.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>fn</code> là một hàm</li>
	<li><code>args</code> là một mảng JSON hợp lệ</li>
	<li><code>1 &lt;= args.length &lt;= 10</code></li>
	<li><code><font face="monospace">20 &lt;= t &lt;= 1000</font></code></li>
	<li><code><font face="monospace">10 &lt;= cancelTimeMs &lt;= 1000</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta phải gọi $fn$ sau $t$ mili giây và cho phép hủy trước khi bộ hẹn giờ kích hoạt. Một cờ được kiểm tra định kỳ vẫn để bộ hẹn giờ hết hạn.
>
> $setTimeout$ lên lịch gọi hàm; hàm hủy được trả về sẽ $clearTimeout$ handle tương ứng. Việc hủy sau khi hàm đã được gọi không làm gì, đúng theo yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function cancellable(fn: Function, args: any[], t: number): Function {
    const timer = setTimeout(() => fn(...args), t);
    return () => {
        clearTimeout(timer);
    };
}

/**
 *  const result = []
 *
 *  const fn = (x) => x * 5
 *  const args = [2], t = 20, cancelT = 50
 *
 *  const start = performance.now()
 *
 *  const log = (...argsArr) => {
 *      const diff = Math.floor(performance.now() - start);
 *      result.push({"time": diff, "returned": fn(...argsArr))
 *  }
 *
 *  const cancel = cancellable(log, args, t);
 *
 *  const maxT = Math.max(t, cancelT)
 *
 *  setTimeout(() => {
 *     cancel()
 *  }, cancelT)
 *
 *  setTimeout(() => {
 *     console.log(result) // [{"time":20,"returned":10}]
 *  }, maxT + 15)
 */
```

#### JavaScript

```js
/**
 * @param {Function} fn
 * @param {Array} args
 * @param {number} t
 * @return {Function}
 */
var cancellable = function (fn, args, t) {
    const timer = setTimeout(() => fn(...args), t);
    return () => {
        clearTimeout(timer);
    };
};

/**
 *  const result = []
 *
 *  const fn = (x) => x * 5
 *  const args = [2], t = 20, cancelT = 50
 *
 *  const start = performance.now()
 *
 *  const log = (...argsArr) => {
 *      const diff = Math.floor(performance.now() - start);
 *      result.push({"time": diff, "returned": fn(...argsArr))
 *  }
 *
 *  const cancel = cancellable(log, args, t);
 *
 *  const maxT = Math.max(t, cancelT)
 *
 *  setTimeout(() => {
 *     cancel()
 *  }, cancelT)
 *
 *  setTimeout(() => {
 *     console.log(result) // [{"time":20,"returned":10}]
 *  }, maxT + 15)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
