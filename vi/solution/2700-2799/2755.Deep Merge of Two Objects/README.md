---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2755. Deep Merge of Two Objects 🔒](https://leetcode.com/problems/deep-merge-of-two-objects)

[中文文档](/solution/2700-2799/2755.Deep%20Merge%20of%20Two%20Objects/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai giá trị&nbsp;<code>obj1</code> và <code>obj2</code>, hãy trả về một giá trị được&nbsp;<strong>deep merge</strong>.</p>

<p>Các giá trị được <strong>deep merge</strong> theo các quy tắc sau:</p>

<ul>
	<li>Nếu hai giá trị là object, object kết quả phải có tất cả các key tồn tại trong một trong hai object.&nbsp;Nếu một key thuộc về cả hai object, hãy <strong>deep merge</strong> hai giá trị tương ứng. Nếu không, hãy thêm cặp key-value đó vào object kết quả.</li>
	<li>Nếu hai giá trị là mảng, mảng kết quả phải có độ dài bằng mảng dài hơn.&nbsp;Áp dụng logic tương tự như với object, nhưng xem các chỉ số là key.</li>
	<li>Nếu không thuộc các trường hợp trên, giá trị kết quả là&nbsp;<code>obj2</code>.</li>
</ul>

<p>Có thể giả sử rằng&nbsp;<code>obj1</code> và <code>obj2</code>&nbsp;là kết quả đầu ra của&nbsp;<code>JSON.parse()</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj1 = {&quot;a&quot;: 1, &quot;c&quot;: 3}, obj2 = {&quot;a&quot;: 2, &quot;b&quot;: 2}
<strong>Đầu ra:</strong> {&quot;a&quot;: 2, &quot;c&quot;: 3, &quot;b&quot;: 2}
<strong>Giải thích:</strong> Giá trị của obj1[&quot;a&quot;] được đổi thành 2 vì nếu cả hai object có cùng key và giá trị của key đó không phải là mảng hoặc object thì giá trị trong obj1 sẽ được đổi thành giá trị trong obj2. Key &quot;b&quot; cùng giá trị của nó được thêm vào obj1 vì key này không tồn tại trong obj1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj1 = [{}, 2, 3], obj2 = [[], 5]
<strong>Đầu ra:</strong> [[], 5, 3]
<strong>Giải thích:</strong> result[0] = obj2[0] vì obj1[0] và obj2[0] có kiểu khác nhau. result[2] = obj1[2] vì obj2[2] không tồn tại.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
obj1 = {&quot;a&quot;: 1, &quot;b&quot;: {&quot;c&quot;: [1 , [2, 7], 5], &quot;d&quot;: 2}},
obj2 = {&quot;a&quot;: 1, &quot;b&quot;: {&quot;c&quot;: [6, [6], [9]], &quot;e&quot;: 3}}
<strong>Đầu ra:</strong> {&quot;a&quot;: 1, &quot;b&quot;: {&quot;c&quot;: [6, [6, 7], [9]], &quot;d&quot;: 2, &quot;e&quot;: 3}}
<strong>Giải thích:</strong>
Các mảng obj1[&quot;b&quot;][&quot;c&quot;] và obj2[&quot;b&quot;][&quot;c&quot;] đã được merge theo cách các giá trị của obj2 ghi đè lên giá trị của obj1 khi đi sâu vào cấu trúc, nhưng chỉ khi chúng không phải là mảng hoặc object.
obj2[&quot;b&quot;][&quot;c&quot;] có key &quot;e&quot; mà obj1 không có nên key này được thêm vào obj1.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj1 = true, obj2 = null
<strong>Đầu ra:</strong> null
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>obj1</code> và <code>obj2</code> là các giá trị JSON hợp lệ</li>
	<li><code>1 &lt;= JSON.stringify(obj1).length &lt;= 5&nbsp;* 10<sup>5</sup></code></li>
	<li><code>1 &lt;= JSON.stringify(obj2).length &lt;= 5&nbsp;* 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Deep merge hai giá trị: đệ quy khi cả hai đều là object hoặc cả hai đều là mảng; khi kiểu dữ liệu không khớp hoặc gặp giá trị vô hướng, giữ $obj2$. Merge nông sẽ không xử lý các object lồng nhau.
>
> Nếu một trong hai phía không phải là object, hoặc một bên là mảng còn bên kia không phải, trả về $obj2$. Ngược lại, duyệt qua các key của $obj2$ và đệ quy trên $obj1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function deepMerge(obj1: any, obj2: any): any {
    const isObj = (obj: any) => obj && typeof obj === 'object';
    const isArr = (obj: any) => Array.isArray(obj);
    if (!isObj(obj1) || !isObj(obj2)) {
        return obj2;
    }
    if (isArr(obj1) !== isArr(obj2)) {
        return obj2;
    }
    for (const key in obj2) {
        obj1[key] = deepMerge(obj1[key], obj2[key]);
    }
    return obj1;
}

/**
 * let obj1 = {"a": 1, "c": 3}, obj2 = {"a": 2, "b": 2};
 * deepMerge(obj1, obj2); // {"a": 2, "c": 3, "b": 2}
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
