---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2984. Find Peak Calling Hours for Each City 🔒](https://leetcode.com/problems/find-peak-calling-hours-for-each-city)

[中文文档](/solution/2900-2999/2984.Find%20Peak%20Calling%20Hours%20for%20Each%20City/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Calls</code></p>

<pre>
+--------------+----------+
| Column Name  | Type     |
+--------------+----------+
| caller_id    | int      |
| recipient_id | int      |
| call_time    | datetime |
| city         | varchar  |
+--------------+----------+
(caller_id, recipient_id, call_time) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng chứa id của người gọi, id của người nhận, thời điểm gọi và thành phố.
</pre>

<p>Viết lời giải để tìm <strong>giờ</strong> gọi <strong>cao điểm</strong> cho mỗi <code>city</code>. Nếu <strong>nhiều giờ</strong> có <strong>cùng</strong> số lượng cuộc gọi, tất cả các giờ đó đều được xem là <strong>giờ cao điểm</strong> của thành phố tương ứng.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo <strong>giờ gọi cao điểm</strong> và </em><code>city</code><em> theo thứ tự <strong>giảm dần</strong></em><em><strong> </strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Calls:
+-----------+--------------+---------------------+----------+
| caller_id | recipient_id | call_time           | city     |
+-----------+--------------+---------------------+----------+
| 8         | 4            | 2021-08-24 22:46:07 | Houston  |
| 4         | 8            | 2021-08-24 22:57:13 | Houston  |
| 5         | 1            | 2021-08-11 21:28:44 | Houston  |
| 8         | 3            | 2021-08-17 22:04:15 | Houston  |
| 11        | 3            | 2021-08-17 13:07:00 | New York |
| 8         | 11           | 2021-08-17 14:22:22 | New York |
+-----------+--------------+---------------------+----------+
<strong>Đầu ra:</strong>
+----------+-------------------+-----------------+
| city     | peak_calling_hour | number_of_calls |
+----------+-------------------+-----------------+
| Houston  | 22                | 3               |
| New York | 14                | 1               |
| New York | 13                | 1               |
+----------+-------------------+-----------------+
<strong>Giải thích:</strong>
Đối với Houston:
  - Giờ cao điểm là 22:00, với tổng cộng 3 cuộc gọi được ghi nhận.
Đối với New York:
  - Hai giờ 13:00 và 14:00 có cùng số lượng cuộc gọi là 1, nên cả hai đều được xem là giờ cao điểm.
Bảng kết quả được sắp xếp theo giờ gọi cao điểm và thành phố theo thứ tự giảm dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi thành phố, cần tìm giờ (hoặc các giờ) có nhiều cuộc gọi nhất và giữ lại cả các trường hợp hòa. Nhóm theo thành phố và $HOUR(call_time)$, sau đó dùng $RANK$ theo số lượng giảm dần và giữ $rk=1$.
>
> Sắp xếp theo giờ và thành phố theo thứ tự giảm dần. Không cần self-join để tìm giá trị lớn nhất.

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
                PARTITION BY city
                ORDER BY cnt DESC
            ) AS rk
        FROM
            (
                SELECT
                    city,
                    HOUR(call_time) AS h,
                    COUNT(1) AS cnt
                FROM Calls
                GROUP BY 1, 2
            ) AS t
    )
SELECT city, h AS peak_calling_hour, cnt AS number_of_calls
FROM T
WHERE rk = 1
ORDER BY 2 DESC, 1 DESC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
