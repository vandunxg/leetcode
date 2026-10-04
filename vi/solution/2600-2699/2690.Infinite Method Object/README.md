---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2690. Infinite Method Object 🔒](https://leetcode.com/problems/infinite-method-object)

[中文文档](/solution/2600-2699/2690.Infinite%20Method%20Object/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm trả về một&nbsp;<strong>object có method</strong><strong>&nbsp;vô hạn</strong>.</p>

<p>Một&nbsp;<strong>object có method</strong><strong>&nbsp;vô hạn</strong>&nbsp;là object cho phép bạn gọi bất kỳ method nào và luôn trả về tên của method đó.</p>

<p>Ví dụ, nếu thực thi&nbsp;<code>obj.abc123()</code>, kết quả sẽ là&nbsp;<code>&quot;abc123&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> method = &quot;abc123&quot;
<strong>Đầu ra:</strong> &quot;abc123&quot;
<strong>Giải thích:</strong>
const obj = createInfiniteObject();
obj[&#39;abc123&#39;](); // &quot;abc123&quot;
Chuỗi được trả về luôn phải trùng với tên method.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> method = &quot;.-qw73n|^2It&quot;
<strong>Đầu ra:</strong> &quot;.-qw73n|^2It&quot;
<strong>Giải thích:</strong> Chuỗi được trả về luôn phải trùng với tên method.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= method.length &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần truy cập thuộc tính phải trả về một hàm tạo ra tên của thuộc tính đó. Không thể liệt kê vô hạn key. Bẫy `get` của `Proxy` sẽ nhận tên thuộc tính và trả về một closure chuyển tên đó thành chuỗi bằng `toString`.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function createInfiniteObject(): Record<string, () => string> {
    return new Proxy(
        {},
        {
            get: (_, prop) => () => prop.toString(),
        },
    );
}

/**
 * const obj = createInfiniteObject();
 * obj['abc123'](); // "abc123"
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
