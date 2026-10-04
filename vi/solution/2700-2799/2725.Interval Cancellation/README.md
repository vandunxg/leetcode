---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2725. Interval Cancellation](https://leetcode.com/problems/interval-cancellation)

[中文文档](/solution/2700-2799/2725.Interval%20Cancellation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm <code>fn</code>, một mảng các đối số&nbsp;<code>args</code> và một khoảng thời gian lặp&nbsp;<code>t</code>, hãy trả về một hàm hủy <code>cancelFn</code>.</p>

<p>Sau khoảng trễ&nbsp;<code>cancelTimeMs</code>, hàm hủy&nbsp;<code>cancelFn</code>&nbsp;được trả về sẽ được gọi.</p>

<pre>
setTimeout(cancelFn, cancelTimeMs)
</pre>

<p>Hàm <code>fn</code> phải được gọi ngay với <code>args</code>, sau đó được gọi lại sau mỗi&nbsp;<code>t</code> mili giây&nbsp;cho đến khi&nbsp;<code>cancelFn</code>&nbsp;được gọi tại thời điểm <code>cancelTimeMs</code> mili giây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (x) =&gt; x * 2, args = [4], t = 35
<strong>Đầu ra:</strong>
[
   {&quot;time&quot;: 0, &quot;returned&quot;: 8},
   {&quot;time&quot;: 35, &quot;returned&quot;: 8},
   {&quot;time&quot;: 70, &quot;returned&quot;: 8},
   {&quot;time&quot;: 105, &quot;returned&quot;: 8},
   {&quot;time&quot;: 140, &quot;returned&quot;: 8},
   {&quot;time&quot;: 175, &quot;returned&quot;: 8}
]
<strong>Giải thích:</strong>
const cancelTimeMs = 190;
const cancelFn = cancellable((x) =&gt; x * 2, [4], 35);
setTimeout(cancelFn, cancelTimeMs);

Cứ sau 35ms, fn(4) được gọi một lần. Đến t=190ms thì hàm bị hủy.
Lần gọi fn thứ 1 ở thời điểm 0ms. fn(4) trả về 8.
Lần gọi fn thứ 2 ở thời điểm 35ms. fn(4) trả về 8.
Lần gọi fn thứ 3 ở thời điểm 70ms. fn(4) trả về 8.
Lần gọi fn thứ 4 ở thời điểm&nbsp;105ms. fn(4) trả về 8.
Lần gọi fn thứ 5 ở thời điểm 140ms. fn(4) trả về 8.
Lần gọi fn thứ 6 ở thời điểm 175ms. fn(4) trả về 8.
Bị hủy ở thời điểm 190ms
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (x1, x2) =&gt; (x1 * x2), args = [2, 5], t = 30
<strong>Đầu ra:</strong>
[
   {&quot;time&quot;: 0, &quot;returned&quot;: 10},
   {&quot;time&quot;: 30, &quot;returned&quot;: 10},
   {&quot;time&quot;: 60, &quot;returned&quot;: 10},
   {&quot;time&quot;: 90, &quot;returned&quot;: 10},
   {&quot;time&quot;: 120, &quot;returned&quot;: 10},
   {&quot;time&quot;: 150, &quot;returned&quot;: 10}
]
<strong>Giải thích:</strong>
const cancelTimeMs = 165;
const cancelFn = cancellable((x1, x2) =&gt; (x1 * x2), [2, 5], 30)
setTimeout(cancelFn, cancelTimeMs)

Cứ sau 30ms, fn(2, 5) được gọi một lần. Đến t=165ms thì hàm bị hủy.
Lần gọi fn thứ 1 ở thời điểm 0ms&nbsp;
Lần gọi fn thứ 2 ở thời điểm 30ms&nbsp;
Lần gọi fn thứ 3 ở thời điểm 60ms&nbsp;
Lần gọi fn thứ 4 ở thời điểm&nbsp;90ms&nbsp;
Lần gọi fn thứ 5 ở thời điểm 120ms&nbsp;
Lần gọi fn thứ 6 ở thời điểm 150ms
Bị hủy ở thời điểm 165ms
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> fn = (x1, x2, x3) =&gt; (x1 + x2 + x3), args = [5, 1, 3], t = 50
<strong>Đầu ra:</strong>
[
   {&quot;time&quot;: 0, &quot;returned&quot;: 9},
   {&quot;time&quot;: 50, &quot;returned&quot;: 9},
   {&quot;time&quot;: 100, &quot;returned&quot;: 9},
   {&quot;time&quot;: 150, &quot;returned&quot;: 9}
]
<strong>Giải thích:</strong>
const cancelTimeMs = 180;
const cancelFn = cancellable((x1, x2, x3) =&gt; (x1 + x2 + x3), [5, 1, 3], 50)
setTimeout(cancelFn, cancelTimeMs)

Cứ sau 50ms, fn(5, 1, 3) được gọi một lần. Đến t=180ms thì hàm bị hủy.
Lần gọi fn thứ 1 ở thời điểm 0ms
Lần gọi fn thứ 2 ở thời điểm 50ms
Lần gọi fn thứ 3 ở thời điểm 100ms
Lần gọi fn thứ 4 ở thời điểm&nbsp;150ms
Bị hủy ở thời điểm 180ms
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>fn</code> là một hàm</li>
	<li><code>args</code> là một mảng JSON hợp lệ</li>
	<li><code>1 &lt;= args.length &lt;= 10</code></li>
	<li><code><font face="monospace">30 &lt;= t &lt;= 100</font></code></li>
	<li><code><font face="monospace">10 &lt;= </font>cancelTimeMs<font face="monospace"> &lt;= 500</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Gọi $fn$ ngay lập tức, sau đó cứ mỗi $t$ mili giây một lần và cho phép hủy bất cứ lúc nào. Nếu nối các lần gọi bằng $setTimeout$, ta sẽ phải tự xóa lần gọi tiếp theo.
>
> Gọi hàm một lần đồng bộ, sau đó dùng $setInterval$ cho cùng một lời gọi. Hàm được trả về sẽ $clearInterval$ handle đó và dừng các lần gọi tiếp theo.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function cancellable(fn: Function, args: any[], t: number): Function {
    fn(...args);
    const time = setInterval(() => fn(...args), t);
    return () => clearInterval(time);
}

/**
 *  const result = []
 *
 *  const fn = (x) => x * 2
 *  const args = [4], t = 20, cancelT = 110
 *
 *  const log = (...argsArr) => {
 *      result.push(fn(...argsArr))
 *  }
 *
 *  const cancel = cancellable(fn, args, t);
 *
 *  setTimeout(() => {
 *     cancel()
 *     console.log(result) // [
 *                         //      {"time":0,"returned":8},
 *                         //      {"time":20,"returned":8},
 *                         //      {"time":40,"returned":8},
 *                         //      {"time":60,"returned":8},
 *                         //      {"time":80,"returned":8},
 *                         //      {"time":100,"returned":8}
 *                         //  ]
 *  }, cancelT)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
