---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1841. League Statistics 🔒](https://leetcode.com/problems/league-statistics)

[中文文档](/solution/1800-1899/1841.League%20Statistics/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Teams</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| team_id        | int     |
| team_name      | varchar |
+----------------+---------+
team_id là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng chứa thông tin về một đội trong giải đấu.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Matches</code></p>

<pre>
+-----------------+---------+
| Column Name     | Type    |
+-----------------+---------+
| home_team_id    | int     |
| away_team_id    | int     |
| home_team_goals | int     |
| away_team_goals | int     |
+-----------------+---------+
(home_team_id, away_team_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về một trận đấu.
home_team_goals là số bàn thắng của đội chủ nhà.
away_team_goals là số bàn thắng của đội khách.
Đội thắng trận là đội có số bàn thắng cao hơn.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo thống kê của giải đấu. Thống kê được xây dựng từ các trận đã đấu, trong đó <strong>đội thắng</strong> nhận <strong>ba điểm</strong> và <strong>đội thua</strong> nhận <strong>không điểm</strong>. Nếu trận đấu kết thúc với <strong>tỉ số hòa</strong>, cả hai đội nhận <strong>một điểm</strong>.</p>

<p>Mỗi hàng của bảng kết quả phải chứa:</p>

<ul>
	<li><code>team_name</code> - Tên của đội trong bảng <code>Teams</code>.</li>
	<li><code>matches_played</code> - Số trận đã đấu với tư cách đội chủ nhà hoặc đội khách.</li>
	<li><code>points</code> - Tổng số điểm đội đã giành được cho đến hiện tại.</li>
	<li><code>goal_for</code> - Tổng số bàn thắng đội ghi được trong tất cả các trận.</li>
	<li><code>goal_against</code> - Tổng số bàn thắng của đối thủ ghi vào lưới đội trong tất cả các trận.</li>
	<li><code>goal_diff</code> - Kết quả của <code>goal_for - goal_against</code>.</li>
</ul>

<p>Trả về bảng kết quả được sắp xếp theo <code>points</code> <strong>giảm dần</strong>. Nếu hai hoặc nhiều đội có cùng <code>points</code>, sắp xếp chúng theo <code>goal_diff</code> <strong>giảm dần</strong>. Nếu vẫn hòa, sắp xếp theo <code>team_name</code> theo <strong>thứ tự từ điển</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Teams:
+---------+-----------+
| team_id | team_name |
+---------+-----------+
| 1       | Ajax      |
| 4       | Dortmund  |
| 6       | Arsenal   |
+---------+-----------+
Bảng Matches:
+--------------+--------------+-----------------+-----------------+
| home_team_id | away_team_id | home_team_goals | away_team_goals |
+--------------+--------------+-----------------+-----------------+
| 1            | 4            | 0               | 1               |
| 1            | 6            | 3               | 3               |
| 4            | 1            | 5               | 2               |
| 6            | 1            | 0               | 0               |
+--------------+--------------+-----------------+-----------------+
<strong>Đầu ra:</strong>
+-----------+----------------+--------+----------+--------------+-----------+
| team_name | matches_played | points | goal_for | goal_against | goal_diff |
+-----------+----------------+--------+----------+--------------+-----------+
| Dortmund  | 2              | 6      | 6        | 2            | 4         |
| Arsenal   | 2              | 2      | 3        | 3            | 0         |
| Ajax      | 4              | 2      | 5        | 9            | -4        |
+-----------+----------------+--------+----------+--------------+-----------+
<strong>Giải thích:</strong>
Ajax (team_id=1) đã đấu 4 trận: thua 2 trận và hòa 2 trận. Tổng điểm = 0 + 0 + 1 + 1 = 2.
Dortmund đã đấu 2 trận: thắng cả 2. Tổng điểm = 3 + 3 = 6.
Arsenal đã đấu 2 trận: hòa cả 2. Tổng điểm = 1 + 1 = 2.
Dortmund đứng đầu bảng. Ajax và Arsenal có cùng số điểm, nhưng Arsenal có goal_diff cao hơn Ajax nên đứng trước Ajax trong bảng.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải tổng hợp số trận, điểm, bàn thắng ghi được/bị thủng và hiệu số bàn thắng cho từng đội, trong đó cách tính cho đội chủ nhà và đội khách khác nhau. Việc nhóm trực tiếp bảng trận đấu không thể biến một trận thành hai hàng tương ứng với hai đội.
>
> $\textit{UNION ALL}$ tách mỗi trận thành một hàng cho đội chủ nhà và một hàng cho đội khách, $\textit{CASE}$ điền điểm và số bàn thắng, sau đó ta nhóm theo $team\_id$, nối với tên đội và sắp xếp theo điểm, hiệu số bàn thắng rồi tên đội.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    Scores AS (
        SELECT
            home_team_id AS team_id,
            CASE
                WHEN home_team_goals > away_team_goals THEN 3
                WHEN home_team_goals < away_team_goals THEN 0
                ELSE 1
            END AS score,
            home_team_goals AS goals,
            away_team_goals AS away_goals
        FROM Matches
        UNION ALL
        SELECT
            away_team_id AS team_id,
            CASE
                WHEN home_team_goals > away_team_goals THEN 0
                WHEN home_team_goals < away_team_goals THEN 3
                ELSE 1
            END AS score,
            away_team_goals AS goals,
            home_team_goals AS away_goals
        FROM Matches
    )
SELECT
    team_name,
    COUNT(1) AS matches_played,
    SUM(score) AS points,
    SUM(goals) AS goal_for,
    SUM(away_goals) AS goal_against,
    (SUM(goals) - SUM(away_goals)) AS goal_diff
FROM
    Scores AS s
    JOIN Teams AS t ON s.team_id = t.team_id
GROUP BY s.team_id
ORDER BY points DESC, goal_diff DESC, team_name;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
