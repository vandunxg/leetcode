---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2795. Parallel Execution of Promises for Individual Results Retrieval 🔒](https://leetcode.com/problems/parallel-execution-of-promises-for-individual-results-retrieval)

[中文文档](/solution/2700-2799/2795.Parallel%20Execution%20of%20Promises%20for%20Individual%20Results%20Retrieval/README.md)

## Mô tả

<!-- description:start -->
<p>Cho một mảng các hàm&nbsp;<code>functions</code>, hãy trả về một promise&nbsp;<code>promise</code>. <code>functions</code>&nbsp;là một mảng các hàm trả về promise&nbsp;<code>fnPromise.</code>&nbsp;Mỗi <code>fnPromise</code>&nbsp;có thể được resolve hoặc reject.&nbsp;&nbsp;</p>

<p>Nếu&nbsp;<code>fnPromise</code> được resolve:</p>

<p>&nbsp; &nbsp; <code>obj = { status: &quot;fulfilled&quot;, value: <em>resolved value</em>}</code></p>

<p>Nếu&nbsp;<code>fnPromise</code> bị reject:</p>

<p>&nbsp; &nbsp;&nbsp;<code>obj = { status: &quot;rejected&quot;, reason: <em>reason of rejection (catched error message)</em>}</code></p>

<p><code>promise</code>&nbsp;phải resolve với một mảng các object&nbsp;<code>obj</code>. Mỗi&nbsp;<code>obj</code>&nbsp;trong mảng phải tương ứng với các promise trong mảng hàm ban đầu, <strong>giữ nguyên thứ tự</strong>.</p>

<p>Hãy thử triển khai mà không sử dụng phương thức dựng sẵn&nbsp;<code>Promise.allSettled()</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [
    () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(15), 100))
]
<strong>Đầu ra: </strong>{&quot;t&quot;:100,&quot;values&quot;:[{&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:15}]}
<strong>Giải thích:</strong>
const time = performance.now()
const promise = promiseAllSettled(functions);
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
promise.then(res =&gt; {
    const out = {t: Math.floor(performance.now() - time), values: res}
    console.log(out) // {&quot;t&quot;:100,&quot;values&quot;:[{&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:15}]}
})

Promise được trả về resolve trong vòng 100 mili giây. Vì promise từ mảng các hàm được fulfill, giá trị resolve của promise được trả về được đặt thành [{&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:15}].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [
    () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(20), 100)),
    () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(15), 100))
]
<strong>Đầu ra:
</strong>{
    &quot;t&quot;:100,
    &quot;values&quot;: [
&nbsp;       {&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:20},
&nbsp;       {&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:15}
    ]
}
<strong>Giải thích:</strong> Promise được trả về resolve trong vòng 100 mili giây, vì thời gian resolve được quyết định bởi promise mất nhiều thời gian nhất để fulfill. Vì các promise từ mảng hàm đều được fulfill, giá trị resolve của promise được trả về được đặt thành [{&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:20},{&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:15}].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [
&nbsp;   () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(30), 200)),
&nbsp;   () =&gt; new Promise((resolve, reject) =&gt; setTimeout(() =&gt; reject(&quot;Error&quot;), 100))
]
<strong>Đầu ra:</strong>
{
    &quot;t&quot;:200,
    &quot;values&quot;: [
        {&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:30},
        {&quot;status&quot;:&quot;rejected&quot;,&quot;reason&quot;:&quot;Error&quot;}
    ]
}
<strong>Giải thích:</strong> Promise được trả về resolve trong vòng 200 mili giây, vì thời gian resolve được quyết định bởi promise mất nhiều thời gian nhất để fulfill. Vì một promise trong mảng hàm được fulfill và một promise khác bị reject, giá trị resolve của promise được trả về là một mảng chứa các object theo thứ tự sau: [{&quot;status&quot;:&quot;fulfilled&quot;,&quot;value&quot;:30}, {&quot;status&quot;:&quot;rejected&quot;,&quot;reason&quot;:&quot;Error&quot;}]. Mỗi object trong mảng tương ứng với các promise trong mảng hàm ban đầu, giữ nguyên thứ tự.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= functions.length &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Triển khai $allSettled$: mọi thành công hoặc thất bại đều trở thành một object trạng thái, promise bên ngoài chờ tất cả hoàn tất và thứ tự khớp với đầu vào. $Promise.all$ sẽ reject ngay khi có lỗi đầu tiên.
>
> Khởi chạy mọi factory song song, ghi một bản ghi $fulfilled$ hoặc $rejected$ tại chỉ số tương ứng, rồi $resolve$ mảng khi bộ đếm cho biết mọi vị trí đã settle.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type FulfilledObj = {
    status: 'fulfilled';
    value: string;
};
type RejectedObj = {
    status: 'rejected';
    reason: string;
};
type Obj = FulfilledObj | RejectedObj;

function promiseAllSettled(functions: Function[]): Promise<Obj[]> {
    return new Promise(resolve => {
        const res: Obj[] = [];
        let count = 0;
        for (let i in functions) {
            functions[i]()
                .then(value => ({ status: 'fulfilled', value }))
                .catch(reason => ({ status: 'rejected', reason }))
                .then(obj => {
                    res[i] = obj;
                    if (++count === functions.length) {
                        resolve(res);
                    }
                });
        }
    });
}

/**
 * const functions = [
 *    () => new Promise(resolve => setTimeout(() => resolve(15), 100))
 * ]
 * const time = performance.now()
 *
 * const promise = promiseAllSettled(functions);
 *
 * promise.then(res => {
 *     const out = {t: Math.floor(performance.now() - time), values: res}
 *     console.log(out) // {"t":100,"values":[{"status":"fulfilled","value":15}]}
 * })
 */
```

#### JavaScript

```js
/**
 * @param {Array<Function>} functions
 * @return {Promise}
 */
var promiseAllSettled = function (functions) {
    return new Promise(resolve => {
        const res = [];
        let count = 0;
        for (let i in functions) {
            functions[i]()
                .then(value => ({ status: 'fulfilled', value }))
                .catch(reason => ({ status: 'rejected', reason }))
                .then(obj => {
                    res[i] = obj;
                    if (++count === functions.length) {
                        resolve(res);
                    }
                });
        }
    });
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
