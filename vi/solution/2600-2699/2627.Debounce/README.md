---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2627. Debounce](https://leetcode.com/problems/debounce)

[中文文档](/solution/2600-2699/2627.Debounce/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm&nbsp;<code>fn</code> và một khoảng thời gian tính bằng mili giây&nbsp;<code>t</code>, hãy trả về một phiên bản&nbsp;<strong>debounced</strong>&nbsp;của hàm đó.</p>

<p>Hàm&nbsp;<strong>debounced</strong>&nbsp;là hàm có việc thực thi bị trì hoãn&nbsp;<code>t</code>&nbsp;mili giây và việc thực thi sẽ bị hủy nếu hàm được gọi lại trong khoảng thời gian đó. Hàm debounced cũng phải nhận các tham số được truyền vào.</p>

<p>Ví dụ, giả sử&nbsp;<code>t = 50ms</code>, và hàm được gọi tại các thời điểm&nbsp;<code>30ms</code>,&nbsp;<code>60ms</code>, và <code>100ms</code>.</p>

<p>2 lần gọi hàm đầu tiên sẽ bị hủy, còn lần gọi thứ 3 sẽ được thực thi tại&nbsp;<code>150ms</code>.</p>

<p>Nếu&nbsp;<code>t = 35ms</code>, lần gọi thứ 1 sẽ bị hủy, lần gọi thứ 2 sẽ được thực thi tại&nbsp;<code>95ms</code>, và lần gọi thứ 3 sẽ được thực thi tại&nbsp;<code>135ms</code>.</p>

<p><img alt="Debounce Schematic" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2627.Debounce/images/screen-shot-2023-04-08-at-11048-pm.png" style="width: 800px; height: 242px;" /></p>

<p>Sơ đồ trên cho thấy debounce biến đổi các sự kiện như thế nào. Mỗi hình chữ nhật biểu thị 100ms và thời gian debounce là 400ms. Mỗi màu biểu thị một nhóm đầu vào khác nhau.</p>

<p>Hãy giải bài toán mà không sử dụng hàm&nbsp;<code>_.debounce()</code>&nbsp;của lodash.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
t = 50
calls = [
&nbsp; {&quot;t&quot;: 50, inputs: [1]},
&nbsp; {&quot;t&quot;: 75, inputs: [2]}
]
<strong>Đầu ra:</strong> [{&quot;t&quot;: 125, inputs: [2]}]
<strong>Giải thích:</strong>
let start = Date.now();
function log(...inputs) {
&nbsp; console.log([Date.now() - start, inputs ])
}
const dlog = debounce(log, 50);
setTimeout(() =&gt; dlog(1), 50);
setTimeout(() =&gt; dlog(2), 75);

Lần gọi thứ 1 bị hủy bởi lần gọi thứ 2 vì lần gọi thứ 2 xảy ra trước 100ms.
Lần gọi thứ 2 bị trì hoãn 50ms và được thực thi tại 125ms. Các đầu vào là (2).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
t = 20
calls = [
&nbsp; {&quot;t&quot;: 50, inputs: [1]},
&nbsp; {&quot;t&quot;: 100, inputs: [2]}
]
<strong>Đầu ra:</strong> [{&quot;t&quot;: 70, inputs: [1]}, {&quot;t&quot;: 120, inputs: [2]}]
<strong>Giải thích:</strong>
Lần gọi thứ 1 bị trì hoãn đến 70ms. Các đầu vào là (1).
Lần gọi thứ 2 bị trì hoãn đến 120ms. Các đầu vào là (2).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
t = 150
calls = [
&nbsp; {&quot;t&quot;: 50, inputs: [1, 2]},
&nbsp; {&quot;t&quot;: 300, inputs: [3, 4]},
&nbsp; {&quot;t&quot;: 300, inputs: [5, 6]}
]
<strong>Đầu ra:</strong> [{&quot;t&quot;: 200, inputs: [1,2]}, {&quot;t&quot;: 450, inputs: [5, 6]}]
<strong>Giải thích:</strong>
Lần gọi thứ 1 bị trì hoãn 150ms và được thực thi tại 200ms. Các đầu vào là (1, 2).
Lần gọi thứ 2 bị hủy bởi lần gọi thứ 3.
Lần gọi thứ 3 bị trì hoãn 150ms và được thực thi tại 450ms. Các đầu vào là (5, 6).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= t &lt;= 1000</code></li>
	<li><code>1 &lt;= calls.length &lt;= 10</code></li>
	<li><code>0 &lt;= calls[i].t &lt;= 1000</code></li>
	<li><code>0 &lt;= calls[i].inputs.length &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các đợt gọi liên tiếp nên được gộp thành một lần gọi, diễn ra $t$ mili giây sau lần kích hoạt cuối cùng. Không thể thực thi ngay lập tức nếu muốn gộp các lần gọi.
>
> Ta giữ timer mới nhất: mỗi lần gọi mới sẽ xóa timer trước đó và khởi động lại timer, sau đó thực thi với các tham số mới nhất.
>
> Closure cũng giữ lại `this`, nhờ đó wrapper có thể hoạt động như một method.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type F = (...p: any[]) => any;

function debounce(fn: F, t: number): F {
    let timeout: ReturnType<typeof setTimeout> | undefined;

    return function (...args) {
        if (timeout !== undefined) {
            clearTimeout(timeout);
        }
        timeout = setTimeout(() => {
            fn.apply(this, args);
        }, t);
    };
}

/**
 * const log = debounce(console.log, 100);
 * log('Hello'); // cancelled
 * log('Hello'); // cancelled
 * log('Hello'); // Logged at t=100ms
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
