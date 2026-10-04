---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2822. Inversion of Object 🔒](https://leetcode.com/problems/inversion-of-object)

[中文文档](/solution/2800-2899/2822.Inversion%20of%20Object/README.md)

## Mô tả

<!-- description:start -->

<p>Với một object hoặc mảng&nbsp;<code>obj</code>, hãy trả về object hoặc mảng đảo ngược&nbsp;<code>invertedObj</code>.</p>

<p><code>invertedObj</code> phải có các key của <code>obj</code> làm value và các value của <code>obj</code> làm key.&nbsp;Các chỉ số của mảng được xem như key.</p>

<p>Hàm phải xử lý các giá trị trùng lặp, nghĩa là nếu có nhiều key trong <code>obj</code> có cùng value, <code>invertedObj</code> phải ánh xạ value đó tới một mảng chứa tất cả các key tương ứng.</p>

<p>Đảm bảo rằng các value trong <code>obj</code> chỉ là các chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = {&quot;a&quot;: &quot;1&quot;, &quot;b&quot;: &quot;2&quot;, &quot;c&quot;: &quot;3&quot;, &quot;d&quot;: &quot;4&quot;}
<strong>Đầu ra:</strong> invertedObj = {&quot;1&quot;: &quot;a&quot;, &quot;2&quot;: &quot;b&quot;, &quot;3&quot;: &quot;c&quot;, &quot;4&quot;: &quot;d&quot;}
<strong>Giải thích:</strong> Các key từ obj trở thành các value trong invertedObj, còn các value từ obj trở thành các key trong invertedObj.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = {&quot;a&quot;: &quot;1&quot;, &quot;b&quot;: &quot;2&quot;, &quot;c&quot;: &quot;2&quot;, &quot;d&quot;: &quot;4&quot;}
<strong>Đầu ra:</strong> invertedObj = {&quot;1&quot;: &quot;a&quot;, &quot;2&quot;: [&quot;b&quot;, &quot;c&quot;], &quot;4&quot;: &quot;d&quot;}
<strong>Giải thích:</strong> Có hai key trong&nbsp;obj&nbsp;cùng một value, nên&nbsp;invertedObj ánh xạ value đó tới một mảng chứa tất cả các key tương ứng.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = [&quot;1&quot;, &quot;2&quot;, &quot;3&quot;, &quot;4&quot;]
<strong>Đầu ra:</strong> invertedObj = {&quot;1&quot;: &quot;0&quot;, &quot;2&quot;: &quot;1&quot;, &quot;3&quot;: &quot;2&quot;, &quot;4&quot;: &quot;3&quot;}
<strong>Giải thích:</strong> Mảng cũng là object, vì vậy mảng đã được chuyển thành object; các key (chỉ số) từ obj trở thành các value trong invertedObj, còn các value từ obj trở thành các key trong invertedObj.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>obj</code> là một object hoặc mảng JSON hợp lệ</li>
	<li><code>typeof obj[key] === &quot;string&quot;</code></li>
	<li><code>2 &lt;= JSON.stringify(obj).length &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khi đảo các cặp key–value, một value có thể tương ứng với nhiều key ban đầu. Lưu một giá trị đơn ở lần xuất hiện đầu tiên và chuyển nó thành một mảng khi value đó xuất hiện lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function invertObject(obj: Record<any, any>): Record<any, any> {
    const ans: Record<any, any> = {};
    for (const key in obj) {
        if (ans.hasOwnProperty(obj[key])) {
            if (Array.isArray(ans[obj[key]])) {
                ans[obj[key]].push(key);
            } else {
                ans[obj[key]] = [ans[obj[key]], key];
            }
        } else {
            ans[obj[key]] = key;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
