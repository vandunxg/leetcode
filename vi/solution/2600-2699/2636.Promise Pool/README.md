---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2636. Promise Pool 🔒](https://leetcode.com/problems/promise-pool)

[中文文档](/solution/2600-2699/2636.Promise%20Pool/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các hàm bất đồng bộ <code>functions</code> và một <strong>giới hạn pool</strong> <code>n</code>, hãy trả về một hàm bất đồng bộ <code>promisePool</code>. Hàm này phải trả về một promise được resolve khi tất cả các hàm đầu vào resolve.</p>

<p><b>Giới hạn pool</b> được định nghĩa là số lượng promise tối đa có thể đang chờ cùng lúc. <code>promisePool</code> phải bắt đầu thực thi nhiều hàm nhất có thể và tiếp tục thực thi các hàm mới khi các promise cũ resolve. <code>promisePool</code> phải thực thi <code>functions[i]</code>, sau đó <code>functions[i + 1]</code>, rồi <code>functions[i + 2]</code>, v.v. Khi promise cuối cùng resolve, <code>promisePool</code> cũng phải resolve.</p>

<p>Ví dụ, nếu <code>n = 1</code>, <code>promisePool</code> sẽ thực thi lần lượt từng hàm. Tuy nhiên, nếu <code>n = 2</code>, trước tiên hàm sẽ thực thi hai function. Khi một trong hai function resolve, function thứ 3 sẽ được thực thi (nếu còn), và tiếp tục như vậy cho đến khi không còn function nào để thực thi.</p>

<p>Bạn có thể giả sử tất cả <code>functions</code> không bao giờ reject. <code>promisePool</code> được phép trả về một promise resolve với bất kỳ giá trị nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
functions = [
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 300)),
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 400)),
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 200))
]
n = 2
<strong>Đầu ra:</strong> [[300,400,500],500]
<strong>Giải thích:</strong>
Có ba function được truyền vào. Chúng lần lượt chờ 300ms, 400ms và 200ms.
Chúng resolve lần lượt tại thời điểm 300ms, 400ms và 500ms. Promise được trả về resolve tại thời điểm 500ms.
Tại t=0, 2 function đầu tiên được thực thi. Đã đạt đến giới hạn pool là 2.
Tại t=300, function thứ 1 resolve và function thứ 3 được thực thi. Kích thước pool là 2.
Tại t=400, function thứ 2 resolve. Không còn function nào để thực thi. Kích thước pool là 1.
Tại t=500, function thứ 3 resolve. Kích thước pool là 0, nên promise được trả về cũng resolve.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:
</strong>functions = [
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 300)),
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 400)),
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 200))
]
n = 5
<strong>Đầu ra:</strong> [[300,400,200],400]
<strong>Giải thích:</strong>
Ba promise đầu vào resolve lần lượt tại thời điểm 300ms, 400ms và 200ms.
Promise được trả về resolve tại thời điểm 400ms.
Tại t=0, cả 3 function đều được thực thi. Giới hạn pool là 5 nhưng không đạt đến.
Tại t=200, function thứ 3 resolve. Kích thước pool là 2.
Tại t=300, function thứ 1 resolve. Kích thước pool là 1.
Tại t=400, function thứ 2 resolve. Kích thước pool là 0, nên promise được trả về cũng resolve.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
functions = [
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 300)),
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 400)),
&nbsp; () =&gt; new Promise(res =&gt; setTimeout(res, 200))
]
n = 1
<strong>Đầu ra:</strong> [[300,700,900],900]
<strong>Giải thích:
</strong>Ba promise đầu vào resolve lần lượt tại thời điểm 300ms, 700ms và 900ms.
Promise được trả về resolve tại thời điểm 900ms.
Tại t=0, function thứ 1 được thực thi. Kích thước pool là 1.
Tại t=300, function thứ 1 resolve và function thứ 2 được thực thi. Kích thước pool là 1.
Tại t=700, function thứ 2 resolve và function thứ 3 được thực thi. Kích thước pool là 1.
Tại t=900, function thứ 3 resolve. Kích thước pool là 0, nên promise được trả về cũng resolve.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= functions.length &lt;= 10</code></li>
	<li><code><font face="monospace">1 &lt;= n &lt;= 10</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta chỉ được chạy đồng thời tối đa $n$ tác vụ async. Gọi `Promise.all` trên mọi function sẽ vượt quá giới hạn.
>
> Khởi chạy $n$ wrapper đầu tiên và đưa phần còn lại vào hàng đợi; khi một tác vụ hoàn tất, lấy tác vụ tiếp theo khỏi hàng đợi cho đến khi không còn tác vụ nào phải chờ.
>
> `Promise.all` chỉ chờ batch ban đầu; các công việc về sau được nối tiếp bởi những `await` đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type F = () => Promise<any>;

function promisePool(functions: F[], n: number): Promise<any> {
    const wrappers = functions.map(fn => async () => {
        await fn();
        const nxt = waiting.shift();
        nxt && (await nxt());
    });

    const running = wrappers.slice(0, n);
    const waiting = wrappers.slice(n);
    return Promise.all(running.map(fn => fn()));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
