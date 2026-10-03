---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2377. Sort the Olympic Table 🔒](https://leetcode.com/problems/sort-the-olympic-table)

[中文文档](/solution/2300-2399/2377.Sort%20the%20Olympic%20Table/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Olympic</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| country       | varchar |
| gold_medals   | int     |
| silver_medals | int     |
| bronze_medals | int     |
+---------------+---------+
Trong SQL, country là khóa chính của bảng này.
Mỗi hàng trong bảng này cho biết tên một quốc gia và số huy chương vàng, bạc và đồng mà quốc gia đó giành được trong Thế vận hội.
</pre>

<p>&nbsp;</p>

<p>Bảng Olympic được sắp xếp theo các quy tắc sau:</p>

<ul>
	<li>Quốc gia có nhiều huy chương vàng hơn được xếp trước.</li>
	<li>Nếu số huy chương vàng bằng nhau, quốc gia có nhiều huy chương bạc hơn được xếp trước.</li>
	<li>Nếu số huy chương bạc bằng nhau, quốc gia có nhiều huy chương đồng hơn được xếp trước.</li>
	<li>Nếu số huy chương đồng cũng bằng nhau, các quốc gia hòa nhau được sắp xếp theo thứ tự từ điển tăng dần.</li>
</ul>

<p>Hãy viết lời giải để sắp xếp bảng Olympic.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Olympic:
+-------------+-------------+---------------+---------------+
| country     | gold_medals | silver_medals | bronze_medals |
+-------------+-------------+---------------+---------------+
| China       | 10          | 10            | 20            |
| South Sudan | 0           | 0             | 1             |
| USA         | 10          | 10            | 20            |
| Israel      | 2           | 2             | 3             |
| Egypt       | 2           | 2             | 2             |
+-------------+-------------+---------------+---------------+
<strong>Đầu ra:</strong>
+-------------+-------------+---------------+---------------+
| country     | gold_medals | silver_medals | bronze_medals |
+-------------+-------------+---------------+---------------+
| China       | 10          | 10            | 20            |
| USA         | 10          | 10            | 20            |
| Israel      | 2           | 2             | 3             |
| Egypt       | 2           | 2             | 2             |
| South Sudan | 0           | 0             | 1             |
+-------------+-------------+---------------+---------------+
<strong>Giải thích:</strong>
China và USA hòa nhau nên được phân định bằng tên theo thứ tự từ điển. Vì "China" đứng trước "USA" theo thứ tự từ điển nên được xếp trước.
Israel đứng trước Egypt vì có nhiều huy chương đồng hơn.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bảng được sắp xếp theo thứ tự giảm dần của huy chương vàng, bạc, đồng, sau đó theo thứ tự tăng dần của quốc gia. Không cần tính thêm giá trị tổng hợp nào.
>
> $ORDER\ BY$ các cột $2,3,4$ theo thứ tự giảm dần và cột $1$ theo thứ tự tăng dần để in toàn bộ bảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT *
FROM Olympic
ORDER BY 2 DESC, 3 DESC, 4 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
