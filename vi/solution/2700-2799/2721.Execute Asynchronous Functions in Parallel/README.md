---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2721. Execute Asynchronous Functions in Parallel](https://leetcode.com/problems/execute-asynchronous-functions-in-parallel)

[中文文档](/solution/2700-2799/2721.Execute%20Asynchronous%20Functions%20in%20Parallel/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các hàm bất đồng bộ&nbsp;<code>functions</code>, hãy trả về một promise mới&nbsp;<code>promise</code>. Mỗi hàm trong mảng không nhận đối số và trả về một promise. Tất cả các promise phải được thực thi song song.</p>

<p><code>promise</code> được resolve:</p>

<ul>
	<li>Khi tất cả promise do&nbsp;<code>functions</code>&nbsp;trả về đều được resolve thành công song song.&nbsp;Giá trị được resolve của&nbsp;<code>promise</code>&nbsp;phải là một mảng chứa tất cả giá trị đã resolve của các promise theo cùng thứ tự như trong&nbsp;<code>functions</code>. <code>promise</code> chỉ được resolve khi tất cả hàm bất đồng bộ trong mảng đã hoàn tất việc thực thi song song.</li>
</ul>

<p><code>promise</code> bị reject:</p>

<ul>
	<li>Khi bất kỳ promise nào do&nbsp;<code>functions</code>&nbsp;trả về bị reject.&nbsp;<code>promise</code> cũng phải bị reject với lý do của lần reject đầu tiên.</li>
</ul>

<p>Hãy giải bài toán mà không sử dụng hàm dựng sẵn&nbsp;<code>Promise.all</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [
&nbsp; () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(5), 200))
]
<strong>Đầu ra:</strong> {&quot;t&quot;: 200, &quot;resolved&quot;: [5]}
<strong>Giải thích:</strong>
promiseAll(functions).then(console.log); // [5]

Hàm duy nhất được resolve ở thời điểm 200ms với giá trị 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [
    () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(1), 200)),
    () =&gt; new Promise((resolve, reject) =&gt; setTimeout(() =&gt; reject(&quot;Error&quot;), 100))
]
<strong>Đầu ra:</strong> {&quot;t&quot;: 100, &quot;rejected&quot;: &quot;Error&quot;}
<strong>Giải thích:</strong> Vì một trong các promise bị reject, promise được trả về cũng bị reject với cùng lỗi tại cùng thời điểm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> functions = [
    () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(4), 50)),
    () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(10), 150)),
    () =&gt; new Promise(resolve =&gt; setTimeout(() =&gt; resolve(16), 100))
]
<strong>Đầu ra:</strong> {&quot;t&quot;: 150, &quot;resolved&quot;: [4, 10, 16]}
<strong>Giải thích:</strong> Tất cả promise đều được resolve thành công. Promise được trả về được resolve khi promise cuối cùng được resolve.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>functions</code>&nbsp;là một mảng các hàm trả về promise</li>
	<li><code>1 &lt;= functions.length &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ngữ nghĩa giống với $Promise.all$: resolve với kết quả theo thứ tự ban đầu hoặc reject khi thất bại đầu tiên. Chờ lần lượt từng hàm sẽ kéo dài latency và che khuất một lỗi reject sớm.
>
> Gọi ngay mọi factory và lưu giá trị tại chỉ số tương ứng. Một bộ đếm theo dõi số lần fulfill và resolve khi bằng độ dài mảng. Bất kỳ rejection nào cũng làm promise ngoài reject; việc ghi theo chỉ số giữ nguyên thứ tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
async function promiseAll<T>(functions: (() => Promise<T>)[]): Promise<T[]> {
    return new Promise<T[]>((resolve, reject) => {
        let cnt = 0;
        const ans = new Array(functions.length);
        for (let i = 0; i < functions.length; ++i) {
            const f = functions[i];
            f()
                .then(res => {
                    ans[i] = res;
                    cnt++;
                    if (cnt === functions.length) {
                        resolve(ans);
                    }
                })
                .catch(err => {
                    reject(err);
                });
        }
    });
}

/**
 * const promise = promiseAll([() => new Promise(res => res(42))])
 * promise.then(console.log); // [42]
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
