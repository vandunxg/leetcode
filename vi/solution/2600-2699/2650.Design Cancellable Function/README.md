---
comments: true
difficulty: Hard
tags:
    - JavaScript
---

<!-- problem:start -->

# [2650. Design Cancellable Function](https://leetcode.com/problems/design-cancellable-function)

[中文文档](/solution/2600-2699/2650.Design%20Cancellable%20Function/README.md)

## Mô tả

<!-- description:start -->
<p>Đôi khi bạn có một task chạy trong thời gian dài và muốn hủy nó trước khi hoàn tất. Để thực hiện điều này, hãy viết một hàm&nbsp;<code>cancellable</code>&nbsp;nhận một generator object và trả về một mảng gồm hai giá trị: một <strong>cancel function</strong> và một <strong>promise</strong>.</p>

<p>Có thể giả sử generator function chỉ&nbsp;yield promises. Hàm của bạn có trách nhiệm truyền các giá trị được promise resolve trở lại generator. Nếu promise reject, hàm của bạn phải throw&nbsp;error đó trở lại generator.</p>

<p>Nếu cancel callback được gọi trước khi generator hoàn tất, hàm của bạn phải throw một error trở lại generator. Error đó phải là chuỗi&nbsp;<code>&quot;Cancelled&quot;</code>&nbsp;(không phải một object <code>Error</code>). Nếu error được catch, promise trả về phải resolve với giá trị tiếp theo được yield hoặc return. Nếu không, promise phải reject với error được throw. Không được thực thi thêm code nào.</p>

<p>Khi generator hoàn tất, promise mà hàm của bạn trả về phải resolve với giá trị generator return. Tuy nhiên, nếu generator throw một error, promise trả về phải reject với error đó.</p>

<p>Ví dụ về cách sử dụng code của bạn:</p>

<pre>
function* tasks() {
  const val = yield new Promise(resolve =&gt; resolve(2 + 2));
  yield new Promise(resolve =&gt; setTimeout(resolve, 100));
  return val + 1; // calculation shouldn&#39;t be done.
}
const [cancel, promise] = cancellable(tasks());
setTimeout(cancel, 50);
promise.catch(console.log); // logs &quot;Cancelled&quot; at t=50ms
</pre>

<p>Nếu&nbsp;<code>cancel()</code>&nbsp;không được gọi hoặc được gọi sau&nbsp;<code>t=100ms</code>, promise sẽ resolve&nbsp;<code>5</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
generatorFunction = function*() {
&nbsp; return 42;
}
cancelledAt = 100
<strong>Đầu ra:</strong> {&quot;resolved&quot;: 42}
<strong>Giải thích:</strong>
const generator = generatorFunction();
const [cancel, promise] = cancellable(generator);
setTimeout(cancel, 100);
promise.then(console.log); // resolves 42 at t=0ms

Generator ngay lập tức yield 42 rồi hoàn tất. Vì vậy, promise trả về ngay lập tức resolve với 42. Lưu ý rằng việc hủy một generator đã hoàn tất sẽ không có tác dụng gì.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
generatorFunction = function*() {
&nbsp; const msg = yield new Promise(res =&gt; res(&quot;Hello&quot;));
&nbsp; throw `Error: ${msg}`;
}
cancelledAt = null
<strong>Đầu ra:</strong> {&quot;rejected&quot;: &quot;Error: Hello&quot;}
<strong>Giải thích:</strong>
Một promise được yield. Hàm xử lý việc này bằng cách chờ promise resolve rồi truyền giá trị đã resolve trở lại generator. Sau đó một error được throw, khiến promise reject với chính error được throw đó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
generatorFunction = function*() {
&nbsp; yield new Promise(res =&gt; setTimeout(res, 200));
&nbsp; return &quot;Success&quot;;
}
cancelledAt = 100
<strong>Đầu ra:</strong> {&quot;rejected&quot;: &quot;Cancelled&quot;}
<strong>Giải thích:</strong>
Trong khi hàm đang chờ promise được yield resolve, cancel() được gọi. Việc này khiến một error được truyền trở lại generator. Vì error này không được catch, promise trả về reject với error đó.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong>
generatorFunction = function*() {
&nbsp; let result = 0;
&nbsp; yield new Promise(res =&gt; setTimeout(res, 100));
&nbsp; result += yield new Promise(res =&gt; res(1));
&nbsp; yield new Promise(res =&gt; setTimeout(res, 100));
&nbsp; result += yield new Promise(res =&gt; res(1));
&nbsp; return result;
}
cancelledAt = null
<strong>Đầu ra:</strong> {&quot;resolved&quot;: 2}
<strong>Giải thích:</strong>
Có 4 promise được yield. Hai trong số đó có giá trị được cộng vào result. Sau 200ms, generator hoàn tất với giá trị 2, và giá trị đó được promise trả về resolve.
</pre>

<p><strong class="example">Ví dụ 5:</strong></p>

<pre>
<strong>Đầu vào:</strong>
generatorFunction = function*() {
&nbsp; let result = 0;
&nbsp; try {
&nbsp;   yield new Promise(res =&gt; setTimeout(res, 100));
&nbsp;   result += yield new Promise(res =&gt; res(1));
&nbsp;   yield new Promise(res =&gt; setTimeout(res, 100));
&nbsp;   result += yield new Promise(res =&gt; res(1));
&nbsp; } catch(e) {
&nbsp;   return result;
&nbsp; }
&nbsp; return result;
}
cancelledAt = 150
<strong>Đầu ra:</strong> {&quot;resolved&quot;: 1}
<strong>Giải thích:</strong>
Hai promise đầu tiên được yield resolve và làm result tăng lên. Tuy nhiên, tại t=150ms, generator bị hủy. Error được truyền vào generator bị catch, result được return và cuối cùng được promise trả về resolve.
</pre>

<p><strong class="example">Ví dụ 6:</strong></p>

<pre>
<strong>Đầu vào:</strong>
generatorFunction = function*() {
&nbsp; try {
&nbsp;   yield new Promise((resolve, reject) =&gt; reject(&quot;Promise Rejected&quot;));
&nbsp; } catch(e) {
&nbsp;   let a = yield new Promise(resolve =&gt; resolve(2));
    let b = yield new Promise(resolve =&gt; resolve(2));
&nbsp;   return a + b;
&nbsp; };
}
cancelledAt = null
<strong>Đầu ra:</strong> {&quot;resolved&quot;: 4}
<strong>Giải thích:</strong>
Promise đầu tiên được yield và ngay lập tức reject. Error này được catch. Vì generator chưa bị hủy, quá trình thực thi tiếp tục như bình thường. Kết quả cuối cùng là resolve với 2 + 2 = 4.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>cancelledAt == null or 0 &lt;= cancelledAt &lt;= 1000</code></li>
	<li><code>generatorFunction</code> trả về một generator object</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Generator yield các Promise và phải có thể hủy. Chỉ await Promise tiếp theo thì không thể inject thao tác hủy.
>
> Một Promise thứ hai reject với `Cancelled` và race với giá trị được yield: kết quả thắng sẽ được truyền vào `next` hoặc `throw`. Cancel function sẽ reject race đó.
>
> Khi generator hoàn tất, giá trị cuối cùng của nó được return.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function cancellable<T>(generator: Generator<Promise<any>, T, unknown>): [() => void, Promise<T>] {
    let cancel: () => void = () => {};
    const cancelPromise = new Promise((resolve, reject) => {
        cancel = () => reject('Cancelled');
    });
    cancelPromise.catch(() => {});

    const promise = (async () => {
        let next = generator.next();
        while (!next.done) {
            try {
                next = generator.next(await Promise.race([next.value, cancelPromise]));
            } catch (e) {
                next = generator.throw(e);
            }
        }
        return next.value;
    })();

    return [cancel, promise];
}

/**
 * function* tasks() {
 *   const val = yield new Promise(resolve => resolve(2 + 2));
 *   yield new Promise(resolve => setTimeout(resolve, 100));
 *   return val + 1;
 * }
 * const [cancel, promise] = cancellable(tasks());
 * setTimeout(cancel, 50);
 * promise.catch(console.log); // logs "Cancelled" at t=50ms
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
