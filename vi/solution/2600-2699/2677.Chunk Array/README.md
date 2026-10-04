---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2677. Chunk Array](https://leetcode.com/problems/chunk-array)

[中文文档](/solution/2600-2699/2677.Chunk%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>arr</code> và kích thước chunk <code>size</code>, hãy trả về một mảng đã được chia thành các <strong>chunk</strong>.</p>

<p>Mảng đã chia thành các <strong>chunk</strong> chứa các phần tử ban đầu trong <code>arr</code>, nhưng gồm các mảng con có độ dài <code>size</code>. Độ dài của mảng con cuối cùng có thể nhỏ hơn <code>size</code> nếu <code>arr.length</code> không chia hết cho <code>size</code>.</p>

<p>Hãy giải bài toán mà không sử dụng hàm <code>_.chunk</code> của lodash.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3,4,5], size = 1
<strong>Đầu ra:</strong> [[1],[2],[3],[4],[5]]
<strong>Giải thích:</strong> Mảng arr được chia thành các mảng con, mỗi mảng có 1 phần tử.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,9,6,3,2], size = 3
<strong>Đầu ra:</strong> [[1,9,6],[3,2]]
<strong>Giải thích:</strong> Mảng arr được chia thành các mảng con có 3 phần tử. Tuy nhiên, chỉ còn lại hai phần tử cho mảng con thứ 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [8,5,3,2,6], size = 6
<strong>Đầu ra:</strong> [[8,5,3,2,6]]
<strong>Giải thích:</strong> Vì size lớn hơn arr.length nên tất cả phần tử đều nằm trong mảng con đầu tiên.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [], size = 1
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không có phần tử nào để chia thành chunk nên trả về một mảng rỗng.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>arr</code> là một chuỗi biểu diễn mảng.</li>
	<li><code>2 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= size &lt;= arr.length + 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng được chia thành các chunk có độ dài $size$, riêng chunk cuối có thể ngắn hơn. Việc tăng chỉ số theo từng bước $size$ và dùng `slice` cho phép phương thức tự xử lý phần cuối, nên không cần trường hợp đặc biệt cho chunk cuối.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function chunk(arr: any[], size: number): any[][] {
    const ans: any[][] = [];
    for (let i = 0, n = arr.length; i < n; i += size) {
        ans.push(arr.slice(i, i + size));
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {Array} arr
 * @param {number} size
 * @return {Array[]}
 */
var chunk = function (arr, size) {
    const ans = [];
    for (let i = 0, n = arr.length; i < n; i += size) {
        ans.push(arr.slice(i, i + size));
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
