---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2314. The First Day of the Maximum Recorded Degree in Each City 🔒](https://leetcode.com/problems/the-first-day-of-the-maximum-recorded-degree-in-each-city)

[中文文档](/solution/2300-2399/2314.The%20First%20Day%20of%20the%20Maximum%20Recorded%20Degree%20in%20Each%20City/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Weather</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| city_id     | int  |
| day         | date |
| degree      | int  |
+-------------+------+
(city_id, day) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng trong bảng này chứa nhiệt độ của một thành phố vào một ngày nhất định.
Tất cả nhiệt độ đều được ghi nhận trong năm 2022.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm ngày có nhiệt độ cao nhất được ghi nhận ở mỗi thành phố. Nếu nhiệt độ cao nhất được ghi nhận nhiều lần ở cùng một thành phố, hãy trả về ngày sớm nhất.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>city_id</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Weather:
+---------+------------+--------+
| city_id | day        | degree |
+---------+------------+--------+
| 1       | 2022-01-07 | -12    |
| 1       | 2022-03-07 | 5      |
| 1       | 2022-07-07 | 24     |
| 2       | 2022-08-07 | 37     |
| 2       | 2022-08-17 | 37     |
| 3       | 2022-02-07 | -7     |
| 3       | 2022-12-07 | -6     |
+---------+------------+--------+
<strong>Đầu ra:</strong>
+---------+------------+--------+
| city_id | day        | degree |
+---------+------------+--------+
| 1       | 2022-07-07 | 24     |
| 2       | 2022-08-07 | 37     |
| 3       | 2022-12-07 | -6     |
+---------+------------+--------+
<strong>Giải thích:</strong>
Với thành phố 1, nhiệt độ cao nhất được ghi nhận vào ngày 2022-07-07, ở mức 24 độ.
Với thành phố 2, nhiệt độ cao nhất được ghi nhận vào ngày 2022-08-07 và 2022-08-17, đều ở mức 37 độ. Ta chọn ngày sớm hơn (2022-08-07).
Với thành phố 3, nhiệt độ cao nhất được ghi nhận vào ngày 2022-12-07, ở mức -6 độ.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi thành phố, cần tìm ngày có nhiệt độ cao nhất; nếu hòa, chọn ngày sớm nhất. Có thể sắp xếp trong từng nhóm, nhưng window function cho phép xếp hạng nhiệt độ và ngày cùng lúc.
>
> Phân vùng theo $city\_id$, sắp xếp theo nhiệt độ giảm dần rồi ngày tăng dần, và giữ lại các hàng có hạng $1$. Cuối cùng, xuất các thành phố theo mã tăng dần.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY city_id
                ORDER BY degree DESC, day
            ) AS rk
        FROM Weather
    )
SELECT city_id, day, degree
FROM T
WHERE rk = 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
