---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2777. Date Range Generator 🔒](https://leetcode.com/problems/date-range-generator)

[中文文档](/solution/2700-2799/2777.Date%20Range%20Generator/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ngày bắt đầu <code>start</code>, một ngày kết thúc <code>end</code> và một số nguyên dương <code>step</code>, hãy trả về một generator object tạo ra các ngày trong khoảng từ <code>start</code> đến <code>end</code>, bao gồm cả hai đầu.</p>

<p>Giá trị của <code>step</code> cho biết số ngày giữa hai giá trị được tạo ra liên tiếp.</p>

<p>Mọi ngày được tạo ra phải có định dạng chuỗi <code>YYYY-MM-DD</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = &quot;2023-04-01&quot;, end = &quot;2023-04-04&quot;, step = 1
<strong>Đầu ra:</strong> [&quot;2023-04-01&quot;,&quot;2023-04-02&quot;,&quot;2023-04-03&quot;,&quot;2023-04-04&quot;]
<strong>Giải thích:</strong>
const g = dateRangeGenerator(start, end, step);
g.next().value // &#39;2023-04-01&#39;
g.next().value // &#39;2023-04-02&#39;
g.next().value // &#39;2023-04-03&#39;
g.next().value // &#39;2023-04-04&#39;</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = &quot;2023-04-10&quot;, end = &quot;2023-04-20&quot;, step = 3
<strong>Đầu ra:</strong> [&quot;2023-04-10&quot;,&quot;2023-04-13&quot;,&quot;2023-04-16&quot;,&quot;2023-04-19&quot;]
<strong>Giải thích:</strong>
const g = dateRangeGenerator(start, end, step);
g.next().value // &#39;2023-04-10&#39;
g.next().value // &#39;2023-04-13&#39;
g.next().value // &#39;2023-04-16&#39;
g.next().value // &#39;2023-04-19&#39;</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = &quot;2023-04-10&quot;, end = &quot;2023-04-10&quot;, step = 1
<strong>Đầu ra:</strong> [&quot;2023-04-10&quot;]
<strong>Giải thích:</strong>
const g = dateRangeGenerator(start, end, step);
g.next().value // &#39;2023-04-10&#39;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>new Date(start) &lt;= new Date(end)</code></li>
	<li><code>start</code> và <code>end</code> đều có định dạng chuỗi <code>YYYY-MM-DD</code></li>
	<li><code>0 &lt;= The difference in days between the start date and the end date &lt;= 1500</code></li>
	<li><code>1 &lt;= step &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sinh ra từng ngày trong một khoảng đóng với bước cố định. Tự thực hiện phép tính ngày Julian dễ sai ở cuối tháng; $Date.setDate$ tự động xử lý việc chuyển ngày.
>
> Duyệt từ ngày bắt đầu đến ngày kết thúc, $yield$ ngày ở dạng ISO, rồi cộng $step$ ngày. Generator tạm dừng sau mỗi lần sinh, vì vậy không cần tạo toàn bộ khoảng ngày trong bộ nhớ.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function* dateRangeGenerator(start: string, end: string, step: number): Generator<string> {
    const startDate = new Date(start);
    const endDate = new Date(end);
    let currentDate = startDate;
    while (currentDate <= endDate) {
        yield currentDate.toISOString().slice(0, 10);
        currentDate.setDate(currentDate.getDate() + step);
    }
}

/**
 * const g = dateRangeGenerator('2023-04-01', '2023-04-04', 1);
 * g.next().value; // '2023-04-01'
 * g.next().value; // '2023-04-02'
 * g.next().value; // '2023-04-03'
 * g.next().value; // '2023-04-04'
 * g.next().done; // true
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
