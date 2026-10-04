---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2796. Repeat String 🔒](https://leetcode.com/problems/repeat-string)

[中文文档](/solution/2700-2799/2796.Repeat%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Viết code mở rộng cho mọi chuỗi để có thể gọi phương thức&nbsp;<code>string.replicate(x)</code>&nbsp;trên bất kỳ chuỗi nào và phương thức này sẽ trả về chuỗi được lặp lại <code>x</code> lần.</p>

<p>Hãy thử triển khai mà không sử dụng phương thức dựng sẵn <code>string.repeat</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> str = &quot;hello&quot;, times = 2
<strong>Đầu ra:</strong> &quot;hellohello&quot;
<strong>Giải thích:</strong> &quot;hello&quot; được lặp lại 2 lần
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> str = &quot;code&quot;, times = 3
<strong>Đầu ra:</strong> &quot;codecodecode&quot;
<strong>Giải thích:</strong> &quot;code&quot; được lặp lại 3 lần
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> str = &quot;js&quot;, times = 1
<strong>Đầu ra:</strong> &quot;js&quot;
<strong>Giải thích:</strong> &quot;js&quot; được lặp lại 1 lần
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= times &lt;=&nbsp;10<sup>5</sup></code></li>
	<li><code>1 &lt;=&nbsp;str.length &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Giả sử, để đơn giản hóa việc phân tích, phép nối chuỗi là một thao tác có thời gian hằng số <code>O(1)</code>. Với giả sử này, bạn có thể viết một thuật toán có độ phức tạp thời gian là <code>O(log n)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thêm một phương thức lặp lại chuỗi $times$ lần. Việc nối chuỗi trong vòng lặp sẽ cấp phát các chuỗi trung gian.
>
> Điền một mảng bằng $times$ bản sao của $this$, rồi $join$ chúng trong một lần cấp phát.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
declare global {
    interface String {
        replicate(times: number): string;
    }
}

String.prototype.replicate = function (times: number) {
    return new Array(times).fill(this).join('');
};
```

#### JavaScript

```js
String.prototype.replicate = function (times) {
    return Array(times).fill(this).join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
