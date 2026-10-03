---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1853. Convert Date Format 🔒](https://leetcode.com/problems/convert-date-format)

[中文文档](/solution/1800-1899/1853.Convert%20Date%20Format/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Days</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| day         | date |
+-------------+------+
day là cột chứa các giá trị duy nhất của bảng này.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để chuyển từng ngày trong <code>Days</code> thành một chuỗi có định dạng <code>&quot;day_name, month_name day, year&quot;</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Days:
+------------+
| day        |
+------------+
| 2022-04-12 |
| 2021-08-09 |
| 2020-06-26 |
+------------+
<strong>Đầu ra:</strong>
+-------------------------+
| day                     |
+-------------------------+
| Tuesday, April 12, 2022 |
| Monday, August 9, 2021  |
| Friday, June 26, 2020   |
+-------------------------+
<strong>Giải thích:</strong> Lưu ý rằng kết quả có phân biệt chữ hoa chữ thường.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ngày tháng phải được in theo thứ trong tuần, tháng, ngày, năm. Không cần join hay filter.
>
> $\textit{DATE\_FORMAT}$ với `%W, %M %e, %Y` cho ra tên đầy đủ của thứ trong tuần, tên đầy đủ của tháng, ngày không có số 0 ở đầu và năm gồm bốn chữ số.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DATE_FORMAT(day, '%W, %M %e, %Y') AS day FROM Days;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
