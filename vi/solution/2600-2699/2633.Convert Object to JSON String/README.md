---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2633. Convert Object to JSON String 🔒](https://leetcode.com/problems/convert-object-to-json-string)

[中文文档](/solution/2600-2699/2633.Convert%20Object%20to%20JSON%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho trước một giá trị, hãy trả về một chuỗi JSON hợp lệ biểu diễn giá trị đó. Giá trị có thể là chuỗi, số, mảng, object, boolean hoặc null.&nbsp;Chuỗi trả về không được chứa khoảng trắng thừa. Thứ tự của các key phải giống với thứ tự do&nbsp;<code>Object.keys()</code> trả về.</p>

<p>Hãy giải bài toán mà không sử dụng phương thức dựng sẵn <code>JSON.stringify</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> object = {&quot;y&quot;:1,&quot;x&quot;:2}
<strong>Đầu ra:</strong> {&quot;y&quot;:1,&quot;x&quot;:2}
<strong>Giải thích:</strong>
Trả về biểu diễn JSON.
Lưu ý rằng thứ tự của các key phải giống với thứ tự do Object.keys() trả về.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> object = {&quot;a&quot;:&quot;str&quot;,&quot;b&quot;:-12,&quot;c&quot;:true,&quot;d&quot;:null}
<strong>Đầu ra:</strong> {&quot;a&quot;:&quot;str&quot;,&quot;b&quot;:-12,&quot;c&quot;:true,&quot;d&quot;:null}
<strong>Giải thích:</strong>
Các kiểu dữ liệu nguyên thủy của JSON là chuỗi, số, boolean và null.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> object = {&quot;key&quot;:{&quot;a&quot;:1,&quot;b&quot;:[{},null,&quot;Hello&quot;]}}
<strong>Đầu ra:</strong> {&quot;key&quot;:{&quot;a&quot;:1,&quot;b&quot;:[{},null,&quot;Hello&quot;]}}
<strong>Giải thích:</strong>
Object và mảng có thể chứa các object và mảng khác.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong> object = true
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Các kiểu dữ liệu nguyên thủy là những đầu vào hợp lệ.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>value</code> là một giá trị JSON hợp lệ</li>
	<li><code>1 &lt;= JSON.stringify(object).length &lt;= 10<sup>5</sup></code></li>
	<li><code>maxNestingLevel &lt;= 1000</code></li>
	<li>tất cả chuỗi chỉ chứa các ký tự chữ và số</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Không được sử dụng `JSON.stringify`, vì vậy mỗi kiểu dữ liệu phải tạo ra văn bản JSON hợp lệ. Các object và mảng lồng nhau cần đệ quy; dữ liệu đầu vào không chứa chu kỳ.
>
> `null`, chuỗi, số và boolean có các literal cố định; mảng và object bao bọc các phần tử đệ quy, còn key sử dụng lại quy tắc xử lý chuỗi.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function jsonStringify(object: any): string {
    if (object === null) {
        return 'null';
    }
    if (typeof object === 'string') {
        return `"${object}"`;
    }
    if (typeof object === 'number' || typeof object === 'boolean') {
        return object.toString();
    }
    if (Array.isArray(object)) {
        return `[${object.map(jsonStringify).join(',')}]`;
    }
    if (typeof object === 'object') {
        return `{${Object.entries(object)
            .map(([key, value]) => `${jsonStringify(key)}:${jsonStringify(value)}`)
            .join(',')}}`;
    }
    return '';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
