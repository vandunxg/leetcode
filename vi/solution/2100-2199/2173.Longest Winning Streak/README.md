---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2173. Longest Winning Streak 🔒](https://leetcode.com/problems/longest-winning-streak)

[中文文档](/solution/2100-2199/2173.Longest%20Winning%20Streak/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Matches</code></p>

<pre>
+-------------+------+
| Tên cột     | Kiểu |
+-------------+------+
| player_id   | int  |
| match_day   | date |
| result      | enum |
+-------------+------+
(player_id, match_day) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này chứa ID của một người chơi, ngày họ thi đấu và kết quả của trận đấu đó.
Cột result có kiểu ENUM (danh mục) gồm (&#39;Win&#39;, &#39;Draw&#39;, &#39;Lose&#39;).
</pre>

<p>&nbsp;</p>

<p><strong>Chuỗi thắng</strong> của một người chơi là số trận thắng liên tiếp, không bị gián đoạn bởi các trận hòa hoặc thua.</p>

<p>Hãy viết lời giải để tính chuỗi thắng dài nhất của mỗi người chơi.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng của bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Matches:
+-----------+------------+--------+
| player_id | match_day  | result |
+-----------+------------+--------+
| 1         | 2022-01-17 | Win    |
| 1         | 2022-01-18 | Win    |
| 1         | 2022-01-25 | Win    |
| 1         | 2022-01-31 | Draw   |
| 1         | 2022-02-08 | Win    |
| 2         | 2022-02-06 | Lose   |
| 2         | 2022-02-08 | Lose   |
| 3         | 2022-03-30 | Win    |
+-----------+------------+--------+
<strong>Đầu ra:</strong>
+-----------+----------------+
| player_id | longest_streak |
+-----------+----------------+
| 1         | 3              |
| 2         | 0              |
| 3         | 1              |
+-----------+----------------+
<strong>Giải thích:</strong>
Người chơi 1:
Từ 2022-01-17 đến 2022-01-25, người chơi 1 thắng 3 trận liên tiếp.
Vào ngày 2022-01-31, người chơi 1 hòa.
Vào ngày 2022-02-08, người chơi 1 thắng một trận.
Chuỗi thắng dài nhất là 3 trận.

Người chơi 2:
Từ 2022-02-06 đến 2022-02-08, người chơi 2 thua 2 trận liên tiếp.
Chuỗi thắng dài nhất là 0 trận.

Người chơi 3:
Vào ngày 2022-03-30, người chơi 3 thắng một trận.
Chuỗi thắng dài nhất là 1 trận.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu chúng ta quan tâm đến việc tính chuỗi dài nhất không thua (tức là thắng hoặc hòa), lời giải sẽ thay đổi như thế nào?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm chuỗi thắng dài nhất của mỗi người chơi. Sau khi sắp xếp theo ngày thi đấu, một chuỗi các kết quả giống nhau có cùng hiệu giữa số thứ tự hàng theo từng người chơi và số thứ tự hàng theo từng cặp (người chơi, kết quả).
>
> Nhóm theo hiệu đó, đếm các hàng có $\textit{result}='Win'$ và lấy giá trị lớn nhất theo từng người chơi.
>
> Các hàm cửa sổ tạo ra các nhóm; truy vấn bên ngoài tổng hợp $\textit{longest\_streak}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT
            *,
            ROW_NUMBER() OVER (
                PARTITION BY player_id
                ORDER BY match_day
            ) - ROW_NUMBER() OVER (
                PARTITION BY player_id, result
                ORDER BY match_day
            ) AS rk
        FROM Matches
    ),
    T AS (
        SELECT player_id, SUM(result = 'Win') AS s
        FROM S
        GROUP BY player_id, rk
    )
SELECT player_id, MAX(s) AS longest_streak
FROM T
GROUP BY player_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
