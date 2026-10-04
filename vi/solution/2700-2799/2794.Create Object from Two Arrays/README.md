---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2794. Create Object from Two Arrays 🔒](https://leetcode.com/problems/create-object-from-two-arrays)

[中文文档](/solution/2700-2799/2794.Create%20Object%20from%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng <code>keysArr</code> và <code>valuesArr</code>, hãy trả về một object mới <code>obj</code>. Mỗi cặp key-value trong <code>obj</code> phải được lấy từ <code>keysArr[i]</code> và <code>valuesArr[i]</code>.</p>

<p>Nếu một key bị trùng với key ở chỉ số trước đó, cặp key-value tương ứng phải được loại bỏ. Nói cách khác, chỉ key xuất hiện đầu tiên được thêm vào object.</p>

<p>Nếu key không phải là chuỗi, hãy chuyển nó thành chuỗi bằng cách gọi <code>String()</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> keysArr = [&quot;a&quot;, &quot;b&quot;, &quot;c&quot;], valuesArr = [1, 2, 3]
<strong>Đầu ra:</strong> {&quot;a&quot;: 1, &quot;b&quot;: 2, &quot;c&quot;: 3}
<strong>Giải thích:</strong> Các key &quot;a&quot;, &quot;b&quot; và &quot;c&quot; lần lượt được ghép với các giá trị 1, 2 và 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> keysArr = [&quot;1&quot;, 1, false], valuesArr = [4, 5, 6]
<strong>Đầu ra:</strong> {&quot;1&quot;: 4, &quot;false&quot;: 6}
<strong>Giải thích:</strong> Trước tiên, tất cả phần tử trong keysArr được chuyển thành chuỗi. Có hai lần xuất hiện của &quot;1&quot;. Giá trị gắn với lần xuất hiện đầu tiên của &quot;1&quot; được sử dụng: 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> keysArr = [], valuesArr = []
<strong>Đầu ra:</strong> {}
<strong>Giải thích:</strong> Vì không có key nào nên trả về một object rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>keysArr</code> và <code>valuesArr</code> là các mảng JSON hợp lệ</li>
	<li><code>2 &lt;= JSON.stringify(keysArr).length,&nbsp;JSON.stringify(valuesArr).length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>keysArr.length === valuesArr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xây dựng một object từ hai mảng key và value song song, chuyển các key thành chuỗi và giữ lại giá trị đầu tiên khi bị trùng. $Object.fromEntries$ sẽ cho phép cặp xuất hiện sau ghi đè lên cặp trước.
>
> Duyệt qua các chỉ số: chuyển key thành chuỗi và chỉ ghi giá trị khi object chưa có key đó, tức giá trị tương ứng vẫn là $undefined$.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function createObject(keysArr: any[], valuesArr: any[]): Record<string, any> {
    const ans: Record<string, any> = {};
    for (let i = 0; i < keysArr.length; ++i) {
        const k = String(keysArr[i]);
        if (ans[k] === undefined) {
            ans[k] = valuesArr[i];
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {Array} keysArr
 * @param {Array} valuesArr
 * @return {Object}
 */
var createObject = function (keysArr, valuesArr) {
    const ans = {};
    for (let i = 0; i < keysArr.length; ++i) {
        const k = keysArr[i] + '';
        if (ans[k] === undefined) {
            ans[k] = valuesArr[i];
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
