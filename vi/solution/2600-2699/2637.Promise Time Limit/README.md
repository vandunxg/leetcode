---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2637. Promise Time Limit](https://leetcode.com/problems/promise-time-limit)

[中文文档](/solution/2600-2699/2637.Promise%20Time%20Limit/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm bất đồng bộ&nbsp;<code>fn</code>&nbsp;và một khoảng thời gian <code>t</code>&nbsp;tính bằng mili giây, hãy trả về một phiên bản&nbsp;<strong>giới hạn thời gian</strong>&nbsp;mới của hàm đầu vào. <code>fn</code> nhận các đối số được truyền vào hàm&nbsp;<strong>giới hạn thời gian</strong>.</p>

<p>Hàm <strong>giới hạn thời gian</strong> phải tuân theo các quy tắc sau:</p>

<ul>
	<li>Nếu <code>fn</code> hoàn tất trong thời gian giới hạn là <code>t</code> mili giây, hàm&nbsp;<strong>giới hạn thời gian</strong>&nbsp;phải resolve với kết quả.</li>
	<li>Nếu việc thực thi <code>fn</code> vượt quá thời gian giới hạn, hàm&nbsp;<strong>giới hạn thời gian</strong>&nbsp;phải reject với chuỗi <code>&quot;Time Limit Exceeded&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = async (n) =&gt; {
&nbsp; await new Promise(res =&gt; setTimeout(res, 100));
&nbsp; return n * n;
}
inputs = [5]
t = 50
<strong>Đầu ra:</strong> {&quot;rejected&quot;:&quot;Time Limit Exceeded&quot;,&quot;time&quot;:50}
<strong>Giải thích:</strong>
const limited = timeLimit(fn, t)
const start = performance.now()
let result;
try {
&nbsp; &nbsp;const res = await limited(...inputs)
&nbsp; &nbsp;result = {&quot;resolved&quot;: res, &quot;time&quot;: Math.floor(performance.now() - start)};
} catch (err) {
&nbsp;  result = {&quot;rejected&quot;: err, &quot;time&quot;: Math.floor(performance.now() - start)};
}
console.log(result) // Output

Hàm được cung cấp sẽ resolve sau 100ms. Tuy nhiên, thời gian giới hạn được đặt là 50ms. Hàm reject tại t=50ms vì đã đạt đến thời gian giới hạn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = async (n) =&gt; {
&nbsp; await new Promise(res =&gt; setTimeout(res, 100));
&nbsp; return n * n;
}
inputs = [5]
t = 150
<strong>Đầu ra:</strong> {&quot;resolved&quot;:25,&quot;time&quot;:100}
<strong>Giải thích:</strong>
Hàm resolve 5 * 5 = 25 tại t=100ms. Thời gian giới hạn không bao giờ đạt đến.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = async (a, b) =&gt; {
&nbsp; await new Promise(res =&gt; setTimeout(res, 120));
&nbsp; return a + b;
}
inputs = [5,10]
t = 150
<strong>Đầu ra:</strong> {&quot;resolved&quot;:15,&quot;time&quot;:120}
<strong>Giải thích:</strong>
Hàm resolve 5 + 10 = 15 tại t=120ms. Thời gian giới hạn không bao giờ đạt đến.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = async () =&gt; {
&nbsp; throw &quot;Error&quot;;
}
inputs = []
t = 1000
<strong>Đầu ra:</strong> {&quot;rejected&quot;:&quot;Error&quot;,&quot;time&quot;:0}
<strong>Giải thích:</strong>
Hàm ngay lập tức throw một error.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= inputs.length &lt;= 10</code></li>
	<li><code>0 &lt;= t &lt;= 1000</code></li>
	<li><code>fn</code> trả về một promise</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Wrapper phải reject nếu hàm ban đầu chạy quá $t$ mili giây. Chỉ dùng `await fn` thì không thể tạo timeout.
>
> `Promise.race` cho hàm chạy đua với một timer reject; promise nào settle trước sẽ thắng. Nội dung reject là literal bắt buộc.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type Fn = (...params: any[]) => Promise<any>;

function timeLimit(fn: Fn, t: number): Fn {
    return async function (...args) {
        return Promise.race([
            fn(...args),
            new Promise((_, reject) => setTimeout(() => reject('Time Limit Exceeded'), t)),
        ]);
    };
}

/**
 * const limited = timeLimit((t) => new Promise(res => setTimeout(res, t)), 100);
 * limited(150).catch(console.log) // "Time Limit Exceeded" at t=100ms
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
