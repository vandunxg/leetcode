---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2775. Undefined to Null 🔒](https://leetcode.com/problems/undefined-to-null)

[中文文档](/solution/2700-2799/2775.Undefined%20to%20Null/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một object hoặc mảng <code>obj</code> lồng nhau nhiều cấp, hãy trả về object <code>obj</code> sau khi thay mọi giá trị <code>undefined</code> bằng <code>null</code>.</p>

<p>Các giá trị <code>undefined</code> được xử lý khác với các giá trị <code>null</code> khi object được chuyển thành chuỗi JSON bằng <code>JSON.stringify()</code>. Hàm này giúp đảm bảo dữ liệu được serialize không chứa lỗi ngoài ý muốn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = {&quot;a&quot;: undefined, &quot;b&quot;: 3}
<strong>Đầu ra:</strong> {&quot;a&quot;: null, &quot;b&quot;: 3}
<strong>Giải thích:</strong> Giá trị của obj.a đã được thay đổi từ undefined thành null
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = {&quot;a&quot;: undefined, &quot;b&quot;: [&quot;a&quot;, undefined]}
<strong>Đầu ra:</strong> {&quot;a&quot;: null,&quot;b&quot;: [&quot;a&quot;, null]}
<strong>Giải thích:</strong> Các giá trị của obj.a và obj.b[1] đã được thay đổi từ undefined thành null
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>obj</code> là một object hoặc mảng JSON hợp lệ</li>
	<li><code>2 &lt;= JSON.stringify(obj).length &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thay $undefined$ bằng $null$ trong object hoặc mảng, bao gồm cả các giá trị lồng nhau. $JSON$ loại bỏ các key có giá trị $undefined$, vì vậy việc serialize rồi parse lại sẽ làm mất chúng.
>
> Duyệt qua mọi key: đệ quy khi giá trị vẫn là một object, sau đó ghi $null$ vào mọi vị trí vẫn còn $undefined$. Các chỉ số của mảng được duyệt bằng cùng vòng lặp $for\cdots in$.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function undefinedToNull(obj: Record<any, any>): Record<any, any> {
    for (const key in obj) {
        if (typeof obj[key] === 'object') {
            obj[key] = undefinedToNull(obj[key]);
        }
        if (obj[key] === undefined) {
            obj[key] = null;
        }
    }
    return obj;
}

/**
 * undefinedToNull({"a": undefined, "b": 3}) // {"a": null, "b": 3}
 * undefinedToNull([undefined, undefined]) // [null, null]
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
