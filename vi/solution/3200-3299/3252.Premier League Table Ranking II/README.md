---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3252. Premier League Table Ranking II 🔒](https://leetcode.com/problems/premier-league-table-ranking-ii)

[中文文档](/solution/3200-3299/3252.Premier%20League%20Table%20Ranking%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>TeamStats</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| team_id          | int     |
| team_name        | varchar |
| matches_played   | int     |
| wins             | int     |
| draws            | int     |
| losses           | int     |
+------------------+---------+
team_id là khóa duy nhất của bảng này.
Bảng này chứa mã đội, tên đội, số trận đã đấu, số trận thắng, số trận hòa và số trận thua.
</pre>

<p>Viết lời giải để tính <strong>điểm</strong>, <strong>vị trí</strong> và <strong>tier</strong> cho mỗi đội trong giải đấu. Điểm được tính như sau:</p>

<ul>
	<li><code>3</code> điểm cho một trận <strong>thắng</strong></li>
	<li><code>1</code> điểm cho một trận <strong>hòa</strong></li>
	<li><code>0</code> điểm cho một trận <strong>thua</strong></li>
</ul>

<p><strong>Lưu ý:</strong>&nbsp;Các đội có cùng số điểm phải được xếp cùng vị trí.</p>

<p><strong>Xếp tier:</strong></p>

<ul>
	<li>Chia giải đấu thành <code>3</code> tier dựa trên số điểm:</li>
	<li>Tier 1: <code>33%</code> số đội đứng đầu</li>
	<li>Tier 2: <code>33%</code> số đội ở giữa</li>
	<li>Tier 3: <code> 34%</code> số đội ở cuối</li>
	<li>Nếu có <strong>các đội hòa điểm</strong> tại <strong>ranh giới tier</strong>, xếp các đội đó vào <strong>tier cao hơn</strong>.</li>
</ul>

<p>Trả về <em>bảng kết quả </em><em>được sắp xếp theo</em> <code>points</code>&nbsp;<em>theo thứ tự&nbsp;<strong>giảm dần</strong>,<strong>&nbsp;</strong>sau đó theo</em> <code>team_name</code> <em>theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả truy vấn như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>TeamStats</code>:</p>

<pre class="example-io">
+---------+-------------------+----------------+------+-------+--------+
| team_id | team_name         | matches_played | wins | draws | losses |
+---------+-------------------+----------------+------+-------+--------+
| 1       | Chelsea           | 22             | 13   | 2     | 7      |
| 2       | Nottingham Forest | 27             | 6    | 6     | 15     |
| 3       | Liverpool         | 17             | 1    | 8     | 8      |
| 4       | Aston Villa       | 20             | 1    | 6     | 13     |
| 5       | Fulham            | 31             | 18   | 1     | 12     |
| 6       | Burnley           | 26             | 6    | 9     | 11     |
| 7       | Newcastle United  | 33             | 11   | 10    | 12     |
| 8       | Sheffield United  | 20             | 18   | 2     | 0      |
| 9       | Luton Town        | 5              | 4    | 0     | 1      |
| 10      | Everton           | 14             | 2    | 6     | 6      |
+---------+-------------------+----------------+------+-------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------------+--------+----------+---------+
| team_name         | points | position | tier    |
+-------------------+--------+----------+---------+
| Sheffield United  | 56     | 1        | Tier 1  |
| Fulham            | 55     | 2        | Tier 1  |
| Newcastle United  | 43     | 3        | Tier 1  |
| Chelsea           | 41     | 4        | Tier 1  |
| Burnley           | 27     | 5        | Tier 2  |
| Nottingham Forest | 24     | 6        | Tier 2  |
| Everton           | 12     | 7        | Tier 2  |
| Luton Town        | 12     | 7        | Tier 2  |
| Liverpool         | 11     | 9        | Tier 3  |
| Aston Villa       | 9      | 10       | Tier 3  |
+-------------------+--------+----------+---------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sheffield United có 56 điểm (18 trận thắng * 3 điểm + 2 trận hòa * 1 điểm) và xếp ở vị trí 1.</li>
	<li>Fulham có 55 điểm (18 trận thắng * 3 điểm + 1 trận hòa * 1 điểm) và xếp ở vị trí 2.</li>
	<li>Newcastle United có 43 điểm (11 trận thắng * 3 điểm + 10 trận hòa * 1 điểm) và xếp ở vị trí 3.</li>
	<li>Chelsea có 41 điểm (13 trận thắng * 3 điểm + 2 trận hòa * 1 điểm) và xếp ở vị trí 4.</li>
	<li>Burnley có 27 điểm (6 trận thắng * 3 điểm + 9 trận hòa * 1 điểm) và xếp ở vị trí 5.</li>
	<li>Nottingham Forest có 24 điểm (6 trận thắng * 3 điểm + 6 trận hòa * 1 điểm) và xếp ở vị trí 6.</li>
	<li>Everton và Luton Town đều có 12 điểm; Everton có 2 trận thắng * 3 điểm + 6 trận hòa * 1 điểm, còn Luton Town có 4 trận thắng * 3 điểm. Cả hai đội cùng xếp ở vị trí 7.</li>
	<li>Liverpool có 11 điểm (1 trận thắng * 3 điểm + 8 trận hòa * 1 điểm) và xếp ở vị trí 9.</li>
	<li>Aston Villa có 9 điểm (1 trận thắng * 3 điểm + 6 trận hòa * 1 điểm) và xếp ở vị trí 10.</li>
</ul>

<p><strong>Cách tính tier:</strong></p>

<ul>
	<li><strong>Tier 1:</strong> 33% số đội đứng đầu dựa trên điểm. Sheffield United, Fulham, Newcastle United và Chelsea thuộc Tier 1.</li>
	<li><strong>Tier 2:</strong> 33% số đội ở giữa. Burnley, Nottingham Forest, Everton và Luton Town thuộc Tier 2.</li>
	<li><strong>Tier 3:</strong> 34% số đội ở cuối. Liverpool và Aston Villa thuộc Tier 3.</li>
</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Window Function + CASE WHEN

<!-- thinking:start -->

> **Tư duy**
>
> Ngoài việc xếp hạng theo điểm, ta chia các đội thành ba tier dựa trên vị trí. Thứ hạng vẫn là min-rank; các mốc chia là một phần ba và hai phần ba bảng, làm tròn lên.
>
> Sau khi tính điểm và `RANK`, một điều kiện lồng nhau gán Tier 1/2/3, rồi sắp xếp theo điểm và tên đội. Window giữ các đội hòa điểm trong cùng tier.

<!-- thinking:end -->

Có thể dùng hàm cửa sổ `RANK()` để tính điểm, thứ hạng và tổng số đội. Sau đó dùng câu lệnh `CASE WHEN` để xác định tier của từng đội.

<!-- tabs:start -->

#### MySQL

```sql
WITH
    T AS (
        SELECT
            team_name,
            wins * 3 + draws AS points,
            RANK() OVER (ORDER BY wins * 3 + draws DESC) AS position,
            COUNT(1) OVER () AS total_teams
        FROM TeamStats
    )
SELECT
    team_name,
    points,
    position,
    CASE
        WHEN position <= CEIL(total_teams / 3.0) THEN 'Tier 1'
        WHEN position <= CEIL(2 * total_teams / 3.0) THEN 'Tier 2'
        ELSE 'Tier 3'
    END tier
FROM T
ORDER BY 2 DESC, 1;
```

#### Pandas

```python
import pandas as pd


def calculate_team_tiers(team_stats: pd.DataFrame) -> pd.DataFrame:
    team_stats["points"] = team_stats["wins"] * 3 + team_stats["draws"]
    team_stats["position"] = (
        team_stats["points"].rank(method="min", ascending=False).astype(int)
    )
    total_teams = len(team_stats)
    team_stats["tier"] = np.where(
        team_stats["position"] <= np.ceil(total_teams / 3.0),
        "Tier 1",
        np.where(
            team_stats["position"] <= np.ceil(2 * total_teams / 3.0), "Tier 2", "Tier 3"
        ),
    )
    team_stats = team_stats.sort_values(
        by=["points", "team_name"], ascending=[False, True]
    )
    return team_stats[["team_name", "points", "position", "tier"]]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
