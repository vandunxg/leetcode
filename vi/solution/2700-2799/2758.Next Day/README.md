---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2758. Next Day 🔒](https://leetcode.com/problems/next-day)

[中文文档](/solution/2700-2799/2758.Next%20Day/README.md)

## Mô tả

<!-- description:start -->

<p>Viết code để mở rộng tất cả đối tượng ngày tháng, cho phép gọi method <code>date.nextDay()</code> trên bất kỳ đối tượng ngày tháng nào và trả về ngày tiếp theo dưới dạng chuỗi theo format <em>YYYY-MM-DD</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> date = &quot;2014-06-20&quot;
<strong>Đầu ra:</strong> &quot;2014-06-21&quot;
<strong>Giải thích:</strong>
const date = new Date(&quot;2014-06-20&quot;);
date.nextDay(); // &quot;2014-06-21&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> date = &quot;2017-10-31&quot;
<strong>Đầu ra:</strong> &quot;2017-11-01&quot;
<strong>Giải thích:</strong> Ngày sau 2017-10-31 là 2017-11-01.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>new Date(date)</code> là một đối tượng ngày tháng hợp lệ</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mở rộng $Date$ bằng một method trả về ngày tiếp theo theo lịch dưới dạng $YYYY\text{-}MM\text{-}DD$. Tự xử lý số ngày trong từng tháng và năm nhuận rất dễ xảy ra sai sót.
>
> Sao chép timestamp, gọi $setDate(getDate()+1)$, rồi lấy phần ngày trong ISO để không thay đổi instance ban đầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
declare global {
    interface Date {
        nextDay(): string;
    }
}

Date.prototype.nextDay = function () {
    const date = new Date(this.valueOf());
    date.setDate(date.getDate() + 1);
    return date.toISOString().slice(0, 10);
};

/**
 * const date = new Date("2014-06-20");
 * date.nextDay(); // "2014-06-21"
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
