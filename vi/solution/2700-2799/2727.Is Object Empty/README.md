---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2727. Is Object Empty](https://leetcode.com/problems/is-object-empty)

[Tài liệu tiếng Trung](/solution/2700-2799/2727.Is%20Object%20Empty/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một object hoặc một mảng, hãy trả về liệu nó có rỗng hay không.</p>

<ul>
	<li>Một object rỗng không chứa cặp key-value nào.</li>
	<li>Một mảng rỗng không chứa phần tử nào.</li>
</ul>

<p>Bạn có thể giả sử object hoặc mảng là kết quả đầu ra của&nbsp;<code>JSON.parse</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = {&quot;x&quot;: 5, &quot;y&quot;: 42}
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Object có 2 cặp key-value nên không rỗng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = {}
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Object không có cặp key-value nào nên rỗng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> obj = [null, false, 0]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Mảng có 3 phần tử nên không rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>obj</code> là một JSON object hoặc mảng hợp lệ</li>
	<li><code>2 &lt;= JSON.stringify(obj).length &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>
<strong>Bạn có thể giải bài toán trong thời gian O(1) không?</strong>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem một object hoặc mảng có không có key enumerable nào. $Object.keys$ kết hợp với việc kiểm tra length là chính xác, nhưng phải tạo ra toàn bộ các key.
>
> Vòng lặp $for\cdots in$ trả về $false$ ngay khi gặp key enumerable đầu tiên và trả về $true$ nếu không có key nào. Với JSON object và mảng, đó chính là việc kiểm tra xem có rỗng hay không.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function isEmpty(obj: Record<string, any> | any[]): boolean {
    for (const x in obj) {
        return false;
    }
    return true;
}
```

#### JavaScript

```js
/**
 * @param {Object | Array} obj
 * @return {boolean}
 */
var isEmpty = function (obj) {
    for (const x in obj) {
        return false;
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dựa trên việc duyệt và dừng sớm. So sánh $Object.keys(obj).length$ với $0$ diễn đạt trực tiếp hơn cùng phép kiểm tra, nhưng phải thu thập các key trước.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function isEmpty(obj: Record<string, any> | any[]): boolean {
    return Object.keys(obj).length === 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
