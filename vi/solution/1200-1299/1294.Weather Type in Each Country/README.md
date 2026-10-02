---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1294. Weather Type in Each Country 🔒](https://leetcode.com/problems/weather-type-in-each-country)

[中文文档](/solution/1200-1299/1294.Weather%20Type%20in%20Each%20Country/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Countries</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| country_id    | int     |
| country_name  | varchar |
+---------------+---------+
country_id là khóa chính của bảng này (cột có giá trị duy nhất).
Mỗi hàng trong bảng chứa ID và tên của một quốc gia.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Weather</code></p>

<pre>
+---------------+------+
| Column Name   | Type |
+---------------+------+
| country_id    | int  |
| weather_state | int  |
| day           | date |
+---------------+------+
(country_id, day) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng trong bảng cho biết trạng thái thời tiết tại một quốc gia vào một ngày.
</pre>

<p>&nbsp;</p>

<p>Viết truy vấn tìm loại thời tiết của mỗi quốc gia trong <strong>tháng 11 năm 2019</strong>.</p>

<p>Phân loại thời tiết như sau:</p>

<ul>
	<li><strong>Cold</strong> nếu giá trị trung bình của <code>weather_state</code> nhỏ hơn hoặc bằng <code>15</code>,</li>
	<li><strong>Hot</strong> nếu giá trị trung bình của <code>weather_state</code> lớn hơn hoặc bằng <code>25</code>, và</li>
	<li><strong>Warm</strong> trong các trường hợp còn lại.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Countries:
+------------+--------------+
| country_id | country_name |
+------------+--------------+
| 2          | USA          |
| 3          | Australia    |
| 7          | Peru         |
| 5          | China        |
| 8          | Morocco      |
| 9          | Spain        |
+------------+--------------+
Bảng Weather:
+------------+---------------+------------+
| country_id | weather_state | day        |
+------------+---------------+------------+
| 2          | 15            | 2019-11-01 |
| 2          | 12            | 2019-10-28 |
| 2          | 12            | 2019-10-27 |
| 3          | -2            | 2019-11-10 |
| 3          | 0             | 2019-11-11 |
| 3          | 3             | 2019-11-12 |
| 5          | 16            | 2019-11-07 |
| 5          | 18            | 2019-11-09 |
| 5          | 21            | 2019-11-23 |
| 7          | 25            | 2019-11-28 |
| 7          | 22            | 2019-12-01 |
| 7          | 20            | 2019-12-02 |
| 8          | 25            | 2019-11-05 |
| 8          | 27            | 2019-11-15 |
| 8          | 31            | 2019-11-25 |
| 9          | 7             | 2019-10-23 |
| 9          | 3             | 2019-12-23 |
+------------+---------------+------------+
<strong>Đầu ra:</strong> 
+--------------+--------------+
| country_name | weather_type |
+--------------+--------------+
| USA          | Cold         |
| Australia    | Cold         |
| Peru         | Hot          |
| Morocco      | Hot          |
| China        | Warm         |
+--------------+--------------+
<strong>Giải thích:</strong> 
Giá trị trung bình của weather_state ở USA trong tháng 11 là (15) / 1 = 15, nên loại thời tiết là Cold.
Giá trị trung bình của weather_state ở Australia trong tháng 11 là (-2 + 0 + 3) / 3 = 0.333, nên loại thời tiết là Cold.
Giá trị trung bình của weather_state ở Peru trong tháng 11 là (25) / 1 = 25, nên loại thời tiết là Hot.
Giá trị trung bình của weather_state ở China trong tháng 11 là (16 + 18 + 21) / 3 = 18.333, nên loại thời tiết là Warm.
Giá trị trung bình của weather_state ở Morocco trong tháng 11 là (25 + 27 + 31) / 3 = 27.667, nên loại thời tiết là Hot.
Do không có dữ liệu để tính weather_state trung bình ở Spain trong tháng 11, nên quốc gia này không xuất hiện trong bảng kết quả. 
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần phân loại nhiệt độ trung bình tháng 11 năm $2019$ của từng quốc gia thành Cold/Warm/Hot. Join bảng thời tiết với bảng quốc gia, lọc theo tháng đó, tính $AVG$ theo quốc gia rồi phân loại bằng $CASE$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    country_name,
    CASE
        WHEN AVG(weather_state) <= 15 THEN 'Cold'
        WHEN AVG(weather_state) >= 25 THEN 'Hot'
        ELSE 'Warm'
    END AS weather_type
FROM
    Weather AS w
    JOIN Countries USING (country_id)
WHERE DATE_FORMAT(day, '%Y-%m') = '2019-11'
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
