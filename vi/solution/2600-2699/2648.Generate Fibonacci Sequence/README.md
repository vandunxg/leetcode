---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2648. Generate Fibonacci Sequence](https://leetcode.com/problems/generate-fibonacci-sequence)

[中文文档](/solution/2600-2699/2648.Generate%20Fibonacci%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm generator trả về một đối tượng generator, đối tượng này lần lượt sinh ra <strong>dãy Fibonacci</strong>.</p>

<p><strong>Dãy Fibonacci</strong> được định nghĩa bởi hệ thức <code>X<sub>n</sub>&nbsp;= X<sub>n-1</sub>&nbsp;+ X<sub>n-2</sub></code>.</p>

<p>Một vài số đầu tiên của dãy là <code>0, 1, 1, 2, 3, 5, 8, 13</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> callCount = 5
<strong>Đầu ra:</strong> [0,1,1,2,3]
<strong>Giải thích:</strong>
const gen = fibGenerator();
gen.next().value; // 0
gen.next().value; // 1
gen.next().value; // 1
gen.next().value; // 2
gen.next().value; // 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> callCount = 0
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> gen.next() không bao giờ được gọi nên không có gì được xuất ra
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= callCount &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Không thể tạo sẵn một stream Fibonacci vô hạn thành một mảng, mà phải tuân theo generator protocol.
>
> Giữ hai số hạng liên tiếp $a,b$, yield $a$, rồi cập nhật cặp này. Generator chỉ tiến lên khi caller yêu cầu lấy giá trị.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function* fibGenerator(): Generator<number, any, number> {
    let a = 0;
    let b = 1;
    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

/**
 * const gen = fibGenerator();
 * gen.next().value; // 0
 * gen.next().value; // 1
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
