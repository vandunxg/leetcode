---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2724. Sort By](https://leetcode.com/problems/sort-by)

[中文文档](/solution/2700-2799/2724.Sort%20By/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>arr</code> và một hàm <code>fn</code>, hãy trả về mảng đã sắp xếp <code>sortedArr</code>. Có thể giả sử&nbsp;<code>fn</code>&nbsp;chỉ trả về các số và những số đó quyết định thứ tự sắp xếp của&nbsp;<code>sortedArr</code>. <code>sortedArr</code> phải được sắp xếp theo <strong>thứ tự tăng dần</strong> dựa trên output của <code>fn</code>.</p>

<p>Có thể giả sử rằng&nbsp;<code>fn</code>&nbsp;sẽ không trả về các số trùng nhau với cùng một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [5, 4, 1, 2, 3], fn = (x) =&gt; x
<strong>Đầu ra:</strong> [1, 2, 3, 4, 5]
<strong>Giải thích:</strong> fn chỉ trả về số được truyền vào nên mảng được sắp xếp theo thứ tự tăng dần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [{&quot;x&quot;: 1}, {&quot;x&quot;: 0}, {&quot;x&quot;: -1}], fn = (d) =&gt; d.x
<strong>Đầu ra:</strong> [{&quot;x&quot;: -1}, {&quot;x&quot;: 0}, {&quot;x&quot;: 1}]
<strong>Giải thích:</strong> fn trả về giá trị của khóa &quot;x&quot;. Vì vậy, mảng được sắp xếp dựa trên giá trị đó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [[3, 4], [5, 2], [10, 1]], fn = (x) =&gt; x[1]
<strong>Đầu ra:</strong> [[10, 1], [5, 2], [3, 4]]
<strong>Giải thích:</strong> arr được sắp xếp theo thứ tự tăng dần dựa trên số tại index=1.&nbsp;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>arr</code> là một mảng JSON hợp lệ</li>
	<li><code>fn</code> là một hàm trả về một số</li>
	<li><code>1 &lt;=&nbsp;arr.length &lt;= 5 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp mảng theo khóa số $fn(item)$. Không cần tự viết comparator hay thêm một stable sort.
>
> Truyền $fn(a)-fn(b)$ cho $sort$ sẽ tạo ra thứ tự cần thiết.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function sortBy(arr: any[], fn: Function): any[] {
    return arr.sort((a, b) => fn(a) - fn(b));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
