---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2693. Call Function with Custom Context](https://leetcode.com/problems/call-function-with-custom-context)

[中文文档](/solution/2600-2699/2693.Call%20Function%20with%20Custom%20Context/README.md)

## Mô tả

<!-- description:start -->

<p>Mở rộng mọi hàm để có phương thức&nbsp;<code>callPolyfill</code>. Phương thức này nhận một object&nbsp;<code>obj</code>&nbsp;làm tham số đầu tiên và một số lượng tùy ý các đối số bổ sung. <code>obj</code>&nbsp;sẽ trở thành context&nbsp;<code>this</code>&nbsp;của hàm. Các đối số bổ sung được truyền vào hàm (hàm mà phương thức&nbsp;<code>callPolyfill</code>&nbsp;thuộc về).</p>

<p>Ví dụ, nếu có hàm:</p>

<pre>
function tax(price, taxRate) {
  const totalCost = price * (1 + taxRate);
&nbsp; console.log(`The cost of ${this.item} is ${totalCost}`);
}
</pre>

<p>Gọi hàm này như&nbsp;<code>tax(10, 0.1)</code>&nbsp;sẽ ghi log&nbsp;<code>&quot;The cost of undefined is 11&quot;</code>. Điều này xảy ra vì context&nbsp;<code>this</code>&nbsp;chưa được xác định.</p>

<p>Tuy nhiên, gọi hàm như&nbsp;<code>tax.callPolyfill({item: &quot;salad&quot;}, 10, 0.1)</code>&nbsp;sẽ ghi log&nbsp;<code>&quot;The cost of salad is 11&quot;</code>. Context&nbsp;<code>this</code>&nbsp;đã được thiết lập chính xác, nên hàm ghi ra kết quả phù hợp.</p>

<p>Hãy giải bài toán này mà không sử dụng phương thức dựng sẵn&nbsp;<code>Function.call</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = function add(b) {
  return this.a + b;
}
args = [{&quot;a&quot;: 5}, 7]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong>
fn.callPolyfill({&quot;a&quot;: 5}, 7); // 12
callPolyfill thiết lập context &quot;this&quot; thành {&quot;a&quot;: 5}. 7 được truyền vào làm đối số.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = function tax(price, taxRate) {
&nbsp;return `The cost of the ${this.item} is ${price * taxRate}`;
}
args = [{&quot;item&quot;: &quot;burger&quot;}, 10, 1.1]
<strong>Đầu ra:</strong> &quot;The cost of the burger is 11&quot;
<strong>Giải thích:</strong> callPolyfill thiết lập context &quot;this&quot; thành {&quot;item&quot;: &quot;burger&quot;}. 10 và 1.1 được truyền vào làm các đối số bổ sung.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code><font face="monospace">typeof args[0] == &#39;object&#39; and args[0] != null</font></code></li>
	<li><code>1 &lt;= args.length &lt;= 100</code></li>
	<li><code>2 &lt;= JSON.stringify(args[0]).length &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần triển khai `call`: chạy hàm với một `this` cho trước. Gắn một thuộc tính tạm thời sẽ làm thay đổi object. `bind(context)` tạo một hàm đã bind; áp dụng các đối số còn lại sẽ tái sử dụng cơ chế bind `this` của engine.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
declare global {
    interface Function {
        callPolyfill(context: Record<any, any>, ...args: any[]): any;
    }
}

Function.prototype.callPolyfill = function (context, ...args): any {
    const fn = this.bind(context);
    return fn(...args);
};

/**
 * function increment() { this.count++; return this.count; }
 * increment.callPolyfill({count: 1}); // 2
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
