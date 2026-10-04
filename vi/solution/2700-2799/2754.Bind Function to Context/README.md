---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2754. Bind Function to Context 🔒](https://leetcode.com/problems/bind-function-to-context)

[中文文档](/solution/2700-2799/2754.Bind%20Function%20to%20Context/README.md)

## Mô tả

<!-- description:start -->

<p>Mở rộng tất cả các hàm để chúng có phương thức&nbsp;<code>bindPolyfill</code>. Khi&nbsp;<code>bindPolyfill</code>&nbsp;được gọi với một object <code>obj</code> được truyền vào, object đó sẽ trở thành context&nbsp;<code>this</code>&nbsp;của hàm.</p>

<p>Ví dụ, với đoạn code:</p>

<pre>
function f() {
  console.log(&#39;My context is &#39; + this.ctx);
}
f();
</pre>

<p>kết quả sẽ là <code>&quot;My context is undefined&quot;</code>. Tuy nhiên, nếu bind hàm:</p>

<pre>
function f() {
  console.log(&#39;My context is &#39; + this.ctx);
}
const boundFunc = f.boundPolyfill({ &quot;ctx&quot;: &quot;My Object&quot; })
boundFunc();
</pre>

<p>kết quả phải là&nbsp;<code>&quot;My context is My Object&quot;</code>.</p>

<p>Bạn có thể giả sử rằng phương thức&nbsp;<code>bindPolyfill</code>&nbsp;sẽ nhận một object khác null.</p>

<p>Hãy giải bài toán mà không sử dụng phương thức dựng sẵn&nbsp;<code>Function.bind</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = function f(multiplier) {
&nbsp; return this.x * multiplier;
}
obj = {&quot;x&quot;: 10}
inputs = [5]
<strong>Đầu ra:</strong> 50
<strong>Giải thích:</strong>
const boundFunc = f.bindPolyfill({&quot;x&quot;: 10});
boundFunc(5); // 50
Truyền multiplier bằng 5 làm tham số.
Context được đặt thành {&quot;x&quot;: 10}.
Nhân hai số này với nhau cho kết quả 50.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = function speak() {
&nbsp; return &quot;My name is &quot; + this.name;
}
obj = {&quot;name&quot;: &quot;Kathy&quot;}
inputs = []
<strong>Đầu ra:</strong> &quot;My name is Kathy&quot;
<strong>Giải thích:</strong>
const boundFunc = f.bindPolyfill({&quot;name&quot;: &quot;Kathy&quot;});
boundFunc(); // &quot;My name is Kathy&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>obj</code> là một object khác null</li>
	<li><code>0 &lt;= inputs.length &lt;= 100</code></li>
</ul>

<p>&nbsp;</p>
<strong>Bạn có thể giải bài toán mà không sử dụng bất kỳ phương thức dựng sẵn nào không?</strong>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Triển khai $bind$: hàm được trả về phải gọi hàm ban đầu với một $this$ xác định. Một wrapper mỏng dựa vào $this$ của chính nó sẽ không cố định được context.
>
> Cài đặt một arrow function trên prototype để $this.call$s $obj$ với các đối số được chuyển tiếp, nhờ đó object được bind vẫn là $obj$.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type Fn = (...args) => any;

declare global {
    interface Function {
        bindPolyfill(obj: Record<any, any>): Fn;
    }
}

Function.prototype.bindPolyfill = function (obj) {
    return (...args) => {
        return this.call(obj, ...args);
    };
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
