---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1783. Grand Slam Titles 🔒](https://leetcode.com/problems/grand-slam-titles)

[中文文档](/solution/1700-1799/1783.Grand%20Slam%20Titles/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Players</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| player_id      | int     |
| player_name    | varchar |
+----------------+---------+
player_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng chứa tên và ID của một vận động viên quần vợt.
</pre>

<p>&nbsp;</p>

<p>Table: <code>Championships</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| year          | int     |
| Wimbledon     | int     |
| Fr_open       | int     |
| US_open       | int     |
| Au_open       | int     |
+---------------+---------+
year là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng chứa ID của các vận động viên đã thắng một giải trong hệ thống Grand Slam.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo số giải Grand Slam mà mỗi vận động viên đã thắng. Không đưa vào những vận động viên chưa thắng giải nào.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Players table:
+-----------+-------------+
| player_id | player_name |
+-----------+-------------+
| 1         | Nadal       |
| 2         | Federer     |
| 3         | Novak       |
+-----------+-------------+
Championships table:
+------+-----------+---------+---------+---------+
| year | Wimbledon | Fr_open | US_open | Au_open |
+------+-----------+---------+---------+---------+
| 2018 | 1         | 1       | 1       | 1       |
| 2019 | 1         | 1       | 2       | 2       |
| 2020 | 2         | 1       | 2       | 2       |
+------+-----------+---------+---------+---------+
<strong>Đầu ra:</strong>
+-----------+-------------+-------------------+
| player_id | player_name | grand_slams_count |
+-----------+-------------+-------------------+
| 2         | Federer     | 5                 |
| 1         | Nadal       | 7                 |
+-----------+-------------+-------------------+
<strong>Giải thích:</strong>
Vận động viên 1 (Nadal) thắng 7 danh hiệu: Wimbledon (2018, 2019), Fr_open (2018, 2019, 2020), US_open (2018) và Au_open (2018).
Vận động viên 2 (Federer) thắng 5 danh hiệu: Wimbledon (2020), US_open (2019, 2020) và Au_open (2019, 2020).
Vận động viên 3 (Novak) không thắng danh hiệu nào nên không được đưa vào bảng kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union All + Equi-Join + Group By

<!-- thinking:start -->

> **Tư duy**
>
> Bốn cột chứa người thắng các giải Grand Slam; ta cần đếm danh hiệu theo từng vận động viên. Chuyển bốn cột thành các hàng, rồi join với bảng vận động viên.
>
> Dùng $\mathrm{UNION\ ALL}$ cho bốn cột ID người thắng, join với $\textit{Players}$, rồi $\mathrm{COUNT}$ theo vận động viên.

<!-- thinking:end -->

Ta có thể dùng `UNION ALL` để gộp tất cả ID của vận động viên thắng các danh hiệu Grand Slam vào bảng `T`, sau đó dùng equi-join `JOIN` để nối bảng `T` với bảng `Players` theo `player_id`, cuối cùng dùng `GROUP BY` và `COUNT` để đếm số danh hiệu Grand Slam của từng vận động viên.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT Wimbledon AS player_id
        FROM Championships
        UNION ALL
        SELECT Fr_open AS player_id
        FROM Championships
        UNION ALL
        SELECT US_open AS player_id
        FROM Championships
        UNION ALL
        SELECT Au_open AS player_id
        FROM Championships
    )
SELECT player_id, player_name, COUNT(1) AS grand_slams_count
FROM
    T
    JOIN Players USING (player_id)
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Thay vì chuyển cột thành hàng, cross join vận động viên với các chức vô địch rồi cộng bốn biểu thức $\mathrm{CASE}$, loại các tổng bằng 0 bằng $\mathrm{HAVING}$. Không cần bảng trung gian; mỗi vận động viên được kiểm tra qua bốn cột.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    player_id,
    player_name,
    SUM(
        (
            CASE
                WHEN Wimbledon = player_id THEN 1
                ELSE 0
            END
        ) + (
            CASE
                WHEN Fr_open = player_id THEN 1
                ELSE 0
            END
        ) + (
            CASE
                WHEN US_open = player_id THEN 1
                ELSE 0
            END
        ) + (
            CASE
                WHEN Au_open = player_id THEN 1
                ELSE 0
            END
        )
    ) AS grand_slams_count
FROM
    Championships
    CROSS JOIN Players
GROUP BY player_id
HAVING grand_slams_count > 0;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
