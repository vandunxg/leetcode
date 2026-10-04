---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2667. Create Hello World Function](https://leetcode.com/problems/create-hello-world-function)

[中文文档](/solution/2600-2699/2667.Create%20Hello%20World%20Function/README.md)

## Mô tả

<!-- description:start -->

Viết một hàm <code>createHelloWorld</code>. Hàm này phải trả về một hàm mới luôn trả về <code>&quot;Hello World&quot;</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> args = []
<strong>Đầu ra:</strong> &quot;Hello World&quot;
<strong>Giải thích:</strong>
const f = createHelloWorld();
f(); // &quot;Hello World&quot;

Hàm được createHelloWorld trả về phải luôn trả về &quot;Hello World&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> args = [{},null,42]
<strong>Đầu ra:</strong> &quot;Hello World&quot;
<strong>Giải thích:</strong>
const f = createHelloWorld();
f({}, null, 42); // &quot;Hello World&quot;

Có thể truyền bất kỳ đối số nào vào hàm, nhưng hàm vẫn phải luôn trả về &quot;Hello World&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= args.length &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Factory phải trả về một hàm bỏ qua mọi đối số và luôn cho ra một chuỗi cố định. Hàm bên trong không đọc `args`, nên mọi đầu vào đều cho cùng một kết quả.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function createHelloWorld() {
    return function (...args): string {
        return 'Hello World';
    };
}

/**
 * const f = createHelloWorld();
 * f(); // "Hello World"
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
