---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2703. Return Length of Arguments Passed](https://leetcode.com/problems/return-length-of-arguments-passed)

[中文文档](/solution/2700-2799/2703.Return%20Length%20of%20Arguments%20Passed/README.md)

## Mô tả

<!-- description:start -->

Viết một hàm&nbsp;<code>argumentsLength</code> trả về số lượng đối số được truyền vào hàm.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> args = [5]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
argumentsLength(5); // 1

Một giá trị được truyền vào hàm nên hàm cần trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> args = [{}, null, &quot;3&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
argumentsLength({}, null, &quot;3&quot;); // 3

Ba giá trị được truyền vào hàm nên hàm cần trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>args</code>&nbsp;là một mảng JSON hợp lệ</li>
	<li><code>0 &lt;= args.length &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nhiệm vụ chỉ là báo cáo có bao nhiêu đối số đã được truyền vào; kiểu và giá trị của chúng không quan trọng. Việc sao chép chúng vào một mảng mới chỉ để đọc độ dài sẽ tạo thêm một lần cấp phát.
>
> Tham số rest vốn đã là một mảng, nên $length$ của nó chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function argumentsLength(...args: any[]): number {
    return args.length;
}

/**
 * argumentsLength(1, 2, 3); // 3
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
