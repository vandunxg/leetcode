---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2649. Nested Array Generator](https://leetcode.com/problems/nested-array-generator)

[中文文档](/solution/2600-2699/2649.Nested%20Array%20Generator/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>mảng đa chiều</strong> gồm các số nguyên, hãy trả về một đối tượng generator tạo ra các số nguyên theo cùng thứ tự như khi <strong>duyệt inorder</strong>.</p>

<p><strong>Mảng đa chiều</strong> là một cấu trúc dữ liệu đệ quy chứa cả số nguyên và các <strong>mảng đa chiều</strong> khác.</p>

<p><strong>Duyệt inorder</strong> lần lượt duyệt từng mảng từ trái sang phải, tạo ra các số nguyên gặp được hoặc áp dụng <strong>duyệt inorder</strong> cho các mảng gặp được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [[[6]],[1,3],[]]
<strong>Đầu ra:</strong> [6,1,3]
<strong>Giải thích:</strong>
const generator = inorderTraversal(arr);
generator.next().value; // 6
generator.next().value; // 1
generator.next().value; // 3
generator.next().done; // true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = []
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không có số nguyên nào nên generator không tạo ra giá trị nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= arr.flat().length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= arr.flat()[i]&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>maxNestingDepth &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>
<strong>Bạn có thể giải bài này mà không tạo một phiên bản đã làm phẳng mới của mảng không?</strong>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các số lồng nhau phải xuất hiện theo thứ tự inorder đã làm phẳng và được tạo ra một cách lazy. Dùng `flat` một lần sẽ làm mất tính lazy. Độ sâu bị giới hạn cho phép dùng `yield*` để nối các generator đệ quy.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type MultidimensionalArray = (MultidimensionalArray | number)[];

function* inorderTraversal(arr: MultidimensionalArray): Generator<number, void, unknown> {
    for (const e of arr) {
        if (Array.isArray(e)) {
            yield* inorderTraversal(e);
        } else {
            yield e;
        }
    }
}

/**
 * const gen = inorderTraversal([1, [2, 3]]);
 * gen.next().value; // 1
 * gen.next().value; // 2
 * gen.next().value; // 3
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
