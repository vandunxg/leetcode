---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2705. Compact Object](https://leetcode.com/problems/compact-object)

[Tài liệu tiếng Trung](/solution/2700-2799/2705.Compact%20Object/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một object hoặc mảng&nbsp;<code>obj</code>, hãy trả về một <strong>compact object</strong>.</p>

<p>Một <strong>compact object</strong>&nbsp;giống với object ban đầu, ngoại trừ các key chứa giá trị <strong>falsy</strong> bị loại bỏ. Thao tác này được áp dụng cho object và mọi object lồng nhau. Mảng được xem là object trong đó các chỉ số là key. Một giá trị được xem là <strong>falsy</strong>&nbsp;khi <code>Boolean(value)</code> trả về <code>false</code>.</p>

<p>Bạn có thể giả sử rằng&nbsp;<code>obj</code> là kết quả của&nbsp;<code>JSON.parse</code>. Nói cách khác, đây là một JSON hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = [null, 0, false, 1]
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong> Tất cả giá trị falsy đã được loại bỏ khỏi mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = {&quot;a&quot;: null, &quot;b&quot;: [false, 1]}
<strong>Đầu ra:</strong> {&quot;b&quot;: [1]}
<strong>Giải thích:</strong> obj[&quot;a&quot;] và obj[&quot;b&quot;][0] chứa các giá trị falsy nên đã bị loại bỏ.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = [null, 0, 5, [0], [false, 16]]
<strong>Đầu ra:</strong> [5, [], [16]]
<strong>Giải thích:</strong> obj[0], obj[1], obj[3][0] và obj[4][0] là các giá trị falsy nên đã bị loại bỏ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>obj</code> là một object JSON hợp lệ</li>
	<li><code>2 &lt;= JSON.stringify(obj).length &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Cần loại bỏ các phần tử falsy khỏi cả object và mảng, bao gồm cả những phần tử lồng nhau. Nếu serialize trước, ta sẽ làm mất ngữ nghĩa của key và không còn phân biệt rõ mảng với object.
>
> Các giá trị không phải object được trả về nguyên trạng. Với mảng, ta lọc các phần tử falsy rồi đệ quy; với object thuần, ta chỉ giữ các giá trị truthy và compact chúng. Object và mảng rỗng đều là truthy nên vẫn được giữ lại, đúng với đề bài.

<!-- thinking:end -->

Nếu `obj` không phải là object hoặc là null, hàm sẽ trả về nguyên trạng vì không cần kiểm tra các key trong những giá trị không phải object.

Nếu `obj` là một mảng, hàm sẽ dùng `obj.filter(Boolean)` để lọc các giá trị falsy (như `null`, `undefined`, `false`, 0, ""), sau đó dùng `map(compactObject)` để gọi đệ quy `compactObject` trên từng phần tử. Nhờ đó, các mảng lồng nhau cũng được compact.

Nếu `obj` là một object, hàm sẽ tạo một object rỗng mới `compactedObj`. Hàm duyệt qua tất cả key của `obj`, với mỗi key sẽ gọi đệ quy `compactObject` trên giá trị tương ứng rồi lưu kết quả vào biến value. Nếu value là truthy (tức là không phải falsy), hàm sẽ gán nó vào object đã compact với key tương ứng.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### TypeScript

```ts
type Obj = Record<any, any>;

function compactObject(obj: Obj): Obj {
    if (!obj || typeof obj !== 'object') {
        return obj;
    }
    if (Array.isArray(obj)) {
        return obj.filter(Boolean).map(compactObject);
    }
    return Object.entries(obj).reduce((acc, [key, value]) => {
        if (value) {
            acc[key] = compactObject(value);
        }
        return acc;
    }, {} as Obj);
}
```

#### JavaScript

```js
/**
 * @param {Object|Array} obj
 * @return {Object|Array}
 */
var compactObject = function (obj) {
    if (!obj || typeof obj !== 'object') {
        return obj;
    }
    if (Array.isArray(obj)) {
        return obj.filter(Boolean).map(compactObject);
    }
    return Object.entries(obj).reduce((acc, [key, value]) => {
        if (value) {
            acc[key] = compactObject(value);
        }
        return acc;
    }, {});
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
