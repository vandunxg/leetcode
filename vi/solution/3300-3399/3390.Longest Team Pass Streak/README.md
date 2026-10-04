---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3390. Longest Team Pass Streak 🔒](https://leetcode.com/problems/longest-team-pass-streak)

[中文文档](/solution/3300-3399/3390.Longest%20Team%20Pass%20Streak/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Teams</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| player_id   | int     |
| team_name   | varchar |
+-------------+---------+
player_id là khóa duy nhất của bảng này.
Mỗi dòng chứa mã định danh duy nhất của cầu thủ và tên của một trong các đội tham gia trận đấu.
</pre>

<p>Bảng: <code>Passes</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| pass_from   | int     |
| time_stamp  | varchar |
| pass_to     | int     |
+-------------+---------+
(pass_from, time_stamp) là khóa duy nhất của bảng này.
pass_from là khóa ngoại tham chiếu đến player_id trong bảng Teams.
Mỗi dòng biểu diễn một đường chuyền được thực hiện trong trận đấu, time_stamp là thời điểm tính bằng phút (00:00-90:00) khi đường chuyền được thực hiện,
pass_to là player_id của cầu thủ nhận đường chuyền.
</pre>

<p>Hãy viết lời giải để tìm <strong>chuỗi chuyền thành công dài nhất</strong> cho <strong>mỗi đội</strong> trong trận đấu. Các quy tắc như sau:</p>

<ul>
    <li>Một chuỗi chuyền thành công được định nghĩa là các đường chuyền liên tiếp trong đó:
    <ul>
        <li>Cả cầu thủ <code>pass_from</code> và <code>pass_to</code> đều thuộc cùng một đội</li>
    </ul>
    </li>
    <li>Một chuỗi bị ngắt khi:
    <ul>
        <li>Đường chuyền bị cắt (được nhận bởi cầu thủ của đội đối phương)</li>
    </ul>
    </li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>team_name</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Teams:</p>

<pre>
+-----------+-----------+
| player_id | team_name |
+-----------+-----------+
| 1         | Arsenal   |
| 2         | Arsenal   |
| 3         | Arsenal   |
| 4         | Arsenal   |
| 5         | Chelsea   |
| 6         | Chelsea   |
| 7         | Chelsea   |
| 8         | Chelsea   |
+-----------+-----------+
</pre>

<p>Bảng Passes:</p>

<pre>
+-----------+------------+---------+
| pass_from | time_stamp | pass_to |
+-----------+------------+---------+
| 1         | 00:05      | 2       |
| 2         | 00:07      | 3       |
| 3         | 00:08      | 4       |
| 4         | 00:10      | 5       |
| 6         | 00:15      | 7       |
| 7         | 00:17      | 8       |
| 8         | 00:20      | 6       |
| 6         | 00:22      | 5       |
| 1         | 00:25      | 2       |
| 2         | 00:27      | 3       |
+-----------+------------+---------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre>
+-----------+----------------+
| team_name | longest_streak |
+-----------+----------------+
| Arsenal   | 3              |
| Chelsea   | 4              |
+-----------+----------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các chuỗi của <strong>Arsenal</strong>:

    <ul>
        <li>Chuỗi đầu tiên: 3 đường chuyền (1&rarr;2&rarr;3&rarr;4), kết thúc khi cầu thủ 4 chuyền cho cầu thủ 5 của Chelsea</li>
        <li>Chuỗi thứ hai: 2 đường chuyền (1&rarr;2&rarr;3)</li>
        <li>Chuỗi dài nhất = 3</li>
    </ul>
    </li>
    <li>Các chuỗi của <strong>Chelsea</strong>:
    <ul>
        <li>Chuỗi đầu tiên: 3 đường chuyền (6&rarr;7&rarr;8&rarr;6&rarr;5)</li>
        <li>Chuỗi dài nhất = 4</li>
    </ul>
    </li>

</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm chuỗi liên tiếp dài nhất gồm các đường chuyền trong cùng đội cho mỗi đội. Một đường chuyền giữa hai đội sẽ ngắt chuỗi của đội đó.
>
> Sau khi join tên đội, tổng tích lũy của $\textit{same\_team}=0$ trong từng đội sẽ chia các đường chuyền thành các nhóm chuỗi.
>
> Chỉ giữ lại các dòng có cùng đội, đếm theo $(\textit{team},\textit{group\_id})$, rồi lấy giá trị lớn nhất cho từng đội.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
WITH
    PassesWithTeams AS (
        SELECT
            p.pass_from,
            p.pass_to,
            t1.team_name AS team_from,
            t2.team_name AS team_to,
            IF(t1.team_name = t2.team_name, 1, 0) same_team_flag,
            p.time_stamp
        FROM
            Passes p
            JOIN Teams t1 ON p.pass_from = t1.player_id
            JOIN Teams t2 ON p.pass_to = t2.player_id
    ),
    StreakGroups AS (
        SELECT
            team_from AS team_name,
            time_stamp,
            same_team_flag,
            SUM(
                CASE
                    WHEN same_team_flag = 0 THEN 1
                    ELSE 0
                END
            ) OVER (
                PARTITION BY team_from
                ORDER BY time_stamp
            ) AS group_id
        FROM PassesWithTeams
    ),
    StreakLengths AS (
        SELECT
            team_name,
            group_id,
            COUNT(*) AS streak_length
        FROM StreakGroups
        WHERE same_team_flag = 1
        GROUP BY 1, 2
    ),
    LongestStreaks AS (
        SELECT
            team_name,
            MAX(streak_length) AS longest_streak
        FROM StreakLengths
        GROUP BY 1
    )
SELECT
    team_name,
    longest_streak
FROM LongestStreaks
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
