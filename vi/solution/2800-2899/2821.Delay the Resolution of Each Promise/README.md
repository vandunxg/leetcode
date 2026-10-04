---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2821. Delay the Resolution of Each Promise 🔒](https://leetcode.com/problems/delay-the-resolution-of-each-promise)

[中文文档](/solution/2800-2899/2821.Delay%20the%20Resolution%20of%20Each%20Promise/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng&nbsp;<code>functions</code>&nbsp;và một số&nbsp;<code>ms</code>, hãy trả về một mảng function mới.</p>

<ul>
	<li><code>functions</code>&nbsp;là một mảng các function trả về promise.</li>
	<li><code>ms</code>&nbsp;biểu thị thời gian trì hoãn tính bằng mili giây. Giá trị này xác định khoảng thời gian cần chờ trước khi resolve hoặc reject mỗi promise trong mảng mới.</li>
</ul>

<p>Mỗi function trong mảng mới phải trả về một promise được resolve hoặc reject sau thêm&nbsp;<code>ms</code>&nbsp;mili giây, đồng thời giữ nguyên thứ tự của mảng&nbsp;<code>functions</code>&nbsp;ban đầu.</p>

<p>Function&nbsp;<code>delayAll</code>&nbsp;phải đảm bảo mỗi promise từ&nbsp;<code>functions</code>&nbsp;được thực thi sau một khoảng trì hoãn, tạo thành mảng mới gồm các function trả về những promise đã được trì hoãn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
functions = [
&nbsp;  () =&gt; new Promise((resolve) =&gt; setTimeout(resolve, 30))
],
ms = 50
<strong>Đầu ra:</strong> [80]
<strong>Giải thích:</strong> Promise trong mảng sẽ được resolve sau 30 mili giây, nhưng bị trì hoãn thêm 50 mili giây, do đó 30 mili giây + 50 mili giây = 80 mili giây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
functions = [
&nbsp;   () =&gt; new Promise((resolve) =&gt; setTimeout(resolve, 50)),
&nbsp;   () =&gt; new Promise((resolve) =&gt; setTimeout(resolve, 80))
],
ms = 70
<strong>Đầu ra:</strong> [120,150]
<strong>Giải thích:</strong> Các promise trong mảng sẽ được resolve sau 50 mili giây và 80 mili giây, nhưng bị trì hoãn thêm 70 mili giây, do đó 50 mili giây + 70 mili giây = 120 mili giây và 80 mili giây + 70 mili giây = 150 mili giây.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
functions = [
&nbsp;   () =&gt; new Promise((resolve, reject) =&gt; setTimeout(reject, 20)),
&nbsp;   () =&gt; new Promise((resolve, reject) =&gt; setTimeout(reject, 100))
],
ms = 30
<strong>Đầu ra: </strong>[50,130]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>functions</code> là một mảng các function trả về promise</li>
	<li><code>10 &lt;= ms &lt;= 500</code></li>
	<li><code>1 &lt;= functions.length &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi function chỉ được bắt đầu sau khi đã chờ $ms$ mili giây. Duyệt qua từng function và tạo một async wrapper, chờ timer rồi mới gọi function ban đầu, nhờ đó giữ nguyên nội dung của function đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function delayAll(functions: Function[], ms: number): Function[] {
    return functions.map(fn => {
        return async function () {
            await new Promise(resolve => setTimeout(resolve, ms));
            return fn();
        };
    });
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
