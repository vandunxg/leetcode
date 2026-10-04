---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2692. Make Object Immutable 🔒](https://leetcode.com/problems/make-object-immutable)

[中文文档](/solution/2600-2699/2692.Make%20Object%20Immutable/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm nhận vào một đối tượng <code>obj</code> và trả về một phiên bản <strong>immutable</strong> mới của đối tượng này.</p>

<p>Một đối tượng <strong>immutable</strong> là đối tượng không thể bị thay đổi và sẽ ném ra lỗi nếu có bất kỳ nỗ lực nào nhằm thay đổi nó.</p>

<p>Đối tượng mới này có thể tạo ra ba loại thông báo lỗi.</p>

<ul>
    <li>Cố gắng thay đổi một key trên object sẽ tạo ra thông báo lỗi: <code>`Error Modifying: ${key}`</code>.</li>
    <li>Cố gắng thay đổi một index trên array sẽ tạo ra thông báo lỗi: <code>`Error Modifying&nbsp;Index: ${index}`</code>.</li>
    <li>Cố gắng gọi một method làm thay đổi array sẽ tạo ra thông báo lỗi: <code>`Error Calling Method: ${methodName}`</code>. Bạn có thể giả sử rằng các method duy nhất có thể làm thay đổi một array là <code>[&#39;pop&#39;, &#39;push&#39;, &#39;shift&#39;, &#39;unshift&#39;, &#39;splice&#39;, &#39;sort&#39;, &#39;reverse&#39;]</code>.</li>
</ul>

<p><code>obj</code> là một object hoặc array JSON hợp lệ, nghĩa là nó là kết quả của <code>JSON.parse()</code>.</p>

<p>Lưu ý rằng phải ném ra một string literal, không phải một <code>Error</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = {
&nbsp; &quot;x&quot;: 5
}
fn = (obj) =&gt; {
&nbsp; obj.x = 5;
&nbsp; return obj.x;
}
<strong>Đầu ra:</strong> {&quot;value&quot;: null, &quot;error&quot;: &quot;Error Modifying:&nbsp;x&quot;}
<strong>Giải thích: </strong>Cố gắng thay đổi một key trên object sẽ dẫn đến lỗi được ném ra. Lưu ý rằng việc giá trị được gán có giống với giá trị trước đó hay không không quan trọng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = [1, 2, 3]
fn = (arr) =&gt; {
&nbsp; arr[1] = {};
&nbsp; return arr[2];
}
<strong>Đầu ra:</strong> {&quot;value&quot;: null, &quot;error&quot;: &quot;Error Modifying&nbsp;Index: 1&quot;}
<strong>Giải thích: </strong>Cố gắng thay đổi một array sẽ dẫn đến lỗi được ném ra.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = {
&nbsp; &quot;arr&quot;: [1, 2, 3]
}
fn = (obj) =&gt; {
&nbsp; obj.arr.push(4);
&nbsp; return 42;
}
<strong>Đầu ra:</strong> { &quot;value&quot;: null, &quot;error&quot;: &quot;Error Calling Method: push&quot;}
<strong>Giải thích: </strong>Gọi một method có thể làm thay đổi array sẽ dẫn đến lỗi được ném ra.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj = {
&nbsp; &quot;x&quot;: 2,
&nbsp; &quot;y&quot;: 2
}
fn = (obj) =&gt; {
&nbsp; return Object.keys(obj);
}
<strong>Đầu ra:</strong> {&quot;value&quot;: [&quot;x&quot;, &quot;y&quot;], &quot;error&quot;: null}
<strong>Giải thích: </strong>Không có thao tác thay đổi nào được thực hiện, nên hàm trả về như bình thường.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>obj</code> là một object hoặc array JSON hợp lệ</li>
    <li><code>2 &lt;= JSON.stringify(obj).length &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Object và array phải từ chối phép gán, đồng thời array cũng phải từ chối các method làm thay đổi nó. `Object.freeze` sẽ thất bại im lặng và không thể bọc `push`.
>
> Đệ quy bọc các giá trị lồng nhau: `set` của object sẽ ném lỗi, còn array sẽ bẫy thêm `pop`/`push` và các method tương tự bằng `apply`. Các phần tử con được bọc trước, sau đó trả về proxy của phần tử gốc.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type Obj = Array<any> | Record<any, any>;

function makeImmutable(obj: Obj): Obj {
    const arrayHandler: ProxyHandler<Array<any>> = {
        set: (_, prop) => {
            throw `Error Modifying Index: ${String(prop)}`;
        },
    };
    const objectHandler: ProxyHandler<Record<any, any>> = {
        set: (_, prop) => {
            throw `Error Modifying: ${String(prop)}`;
        },
    };
    const fnHandler: ProxyHandler<Function> = {
        apply: target => {
            throw `Error Calling Method: ${target.name}`;
        },
    };
    const fn = ['pop', 'push', 'shift', 'unshift', 'splice', 'sort', 'reverse'];
    const dfs = (obj: Obj) => {
        for (const key in obj) {
            if (typeof obj[key] === 'object' && obj[key] !== null) {
                obj[key] = dfs(obj[key]);
            }
        }
        if (Array.isArray(obj)) {
            fn.forEach(f => (obj[f] = new Proxy(obj[f], fnHandler)));
            return new Proxy(obj, arrayHandler);
        }
        return new Proxy(obj, objectHandler);
    };
    return dfs(obj);
}

/**
 * const obj = makeImmutable({x: 5});
 * obj.x = 6; // throws "Error Modifying x"
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
