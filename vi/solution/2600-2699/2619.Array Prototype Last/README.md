---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2619. Array Prototype Last](https://leetcode.com/problems/array-prototype-last)

[中文文档](/solution/2600-2699/2619.Array%20Prototype%20Last/README.md)

## Mô tả

<!-- description:start -->

<p>Viết code để mở rộng tất cả các mảng, cho phép gọi phương thức&nbsp;<code>array.last()</code>&nbsp;trên bất kỳ mảng nào và trả về phần tử cuối cùng. Nếu mảng không có phần tử nào, phương thức phải trả về&nbsp;<code>-1</code>.</p>

<p>Bạn có thể giả sử mảng là kết quả của&nbsp;<code>JSON.parse</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [null, {}, 3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Việc gọi nums.last() phải trả về phần tử cuối cùng: 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = []
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Vì không có phần tử nào, trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>arr</code> là một mảng JSON hợp lệ</li>
	<li><code>0 &lt;= arr.length &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng cần một cách truy cập phần tử cuối; trường hợp mảng rỗng trả về $-1$. Việc sao chép mảng rồi lấy phần tử cuối là lãng phí và làm thay đổi mảng.
>
> Chỉ số cuối cùng là $n-1$, vì vậy `at(-1)` sẽ đọc phần tử đó; với mảng rỗng, ta dùng giá trị đặc biệt.
>
> Gắn phương thức vào `Array.prototype` giúp mọi đối tượng đều có thể sử dụng phương thức này.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
declare global {
    interface Array<T> {
        last(): T | -1;
    }
}

Array.prototype.last = function () {
    return this.length ? this.at(-1) : -1;
};

/**
 * const arr = [1, 2, 3];
 * arr.last(); // 3
 */

export {};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
