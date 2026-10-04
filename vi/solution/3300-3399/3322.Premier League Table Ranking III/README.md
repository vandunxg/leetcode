---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3322. Premier League Table Ranking III 🔒](https://leetcode.com/problems/premier-league-table-ranking-iii)

[中文文档](/solution/3300-3399/3322.Premier%20League%20Table%20Ranking%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>SeasonStats</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| season_id        | int     |
| team_id          | int     |
| team_name        | varchar |
| matches_played   | int     |
| wins             | int     |
| draws            | int     |
| losses           | int     |
| goals_for        | int     |
| goals_against    | int     |
+------------------+---------+
(season_id, team_id) là khóa duy nhất của bảng này.
Bảng này chứa mã mùa giải, mã đội, tên đội, số trận đã đấu, số trận thắng, hòa, thua, số bàn thắng (goals_for) và số bàn thua (goals_against) của mỗi đội trong từng mùa giải.
</pre>

<p>Hãy viết lời giải để tính <strong>điểm số</strong>, <strong>hiệu số bàn thắng</strong> và <b>vị trí&nbsp;</b>cho <strong>mỗi đội</strong> trong <strong>từng mùa giải</strong>. Thứ hạng được xác định như sau:</p>

<ul>
    <li>Các đội trước tiên được xếp hạng theo tổng điểm (từ cao xuống thấp)</li>
    <li>Nếu bằng điểm, các đội được xếp hạng theo hiệu số bàn thắng (từ cao xuống thấp)</li>
    <li>Nếu hiệu số bàn thắng cũng bằng nhau, các đội được xếp hạng theo tên đội theo thứ tự bảng chữ cái</li>
</ul>

<p>Điểm số được tính như sau:</p>

<ul>
    <li><code>3</code> điểm cho một trận <strong>thắng</strong></li>
    <li><code>1</code> điểm cho một trận <strong>hòa</strong></li>
    <li><code>0</code> điểm cho một trận <strong>thua</strong></li>
</ul>

<p>Hiệu số bàn thắng được tính bằng: <code>goals_for - goals_against</code></p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>season_id</code> <em>theo thứ tự <strong>tăng dần</strong>, sau đó theo</em>&nbsp;<font face="monospace">position&nbsp;</font><em>theo thứ tự <strong>tăng dần</strong>, và cuối cùng theo</em> <code>team_name</code> <em>theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng của kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>SeasonStats</code>:</p>

<pre>
+------------+---------+-------------------+----------------+------+-------+--------+-----------+---------------+
| season_id  | team_id | team_name         | matches_played | wins | draws | losses | goals_for | goals_against |
+------------+---------+-------------------+----------------+------+-------+--------+-----------+---------------+
| 2021       | 1       | Manchester City   | 38             | 29   | 6     | 3      | 99        | 26            |
| 2021       | 2       | Liverpool         | 38             | 28   | 8     | 2      | 94        | 26            |
| 2021       | 3       | Chelsea           | 38             | 21   | 11    | 6      | 76        | 33            |
| 2021       | 4       | Tottenham         | 38             | 22   | 5     | 11     | 69        | 40            |
| 2021       | 5       | Arsenal           | 38             | 22   | 3     | 13     | 61        | 48            |
| 2022       | 1       | Manchester City   | 38             | 28   | 5     | 5      | 94        | 33            |
| 2022       | 2       | Arsenal           | 38             | 26   | 6     | 6      | 88        | 43            |
| 2022       | 3       | Manchester United | 38             | 23   | 6     | 9      | 58        | 43            |
| 2022       | 4       | Newcastle         | 38             | 19   | 14    | 5      | 68        | 33            |
| 2022       | 5       | Liverpool         | 38             | 19   | 10    | 9      | 75        | 47            |
+------------+---------+-------------------+----------------+------+-------+--------+-----------+---------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre>
+------------+---------+-------------------+--------+-----------------+----------+
| season_id  | team_id | team_name         | points | goal_difference | position |
+------------+---------+-------------------+--------+-----------------+----------+
| 2021       | 1       | Manchester City   | 93     | 73              | 1        |
| 2021       | 2       | Liverpool         | 92     | 68              | 2        |
| 2021       | 3       | Chelsea           | 74     | 43              | 3        |
| 2021       | 4       | Tottenham         | 71     | 29              | 4        |
| 2021       | 5       | Arsenal           | 69     | 13              | 5        |
| 2022       | 1       | Manchester City   | 89     | 61              | 1        |
| 2022       | 2       | Arsenal           | 84     | 45              | 2        |
| 2022       | 3       | Manchester United | 75     | 15              | 3        |
| 2022       | 4       | Newcastle         | 71     | 35              | 4        |
| 2022       | 5       | Liverpool         | 67     | 28              | 5        |
+------------+---------+-------------------+--------+-----------------+----------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Đối với mùa giải 2021:
    <ul>
        <li>Manchester City có 93 điểm (29 * 3 + 6 * 1) và hiệu số bàn thắng là 73 (99 - 26).</li>
        <li>Liverpool có 92 điểm (28 * 3 + 8 * 1) và hiệu số bàn thắng là 68 (94 - 26).</li>
        <li>Chelsea có 74 điểm (21 * 3 + 11 * 1) và hiệu số bàn thắng là 43 (76 - 33).</li>
        <li>Tottenham có 71 điểm (22 * 3 + 5 * 1) và hiệu số bàn thắng là 29 (69 - 40).</li>
        <li>Arsenal có 69 điểm (22 * 3 + 3 * 1) và hiệu số bàn thắng là 13 (61 - 48).</li>
    </ul>
    </li>
    <li>Đối với mùa giải 2022:
    <ul>
        <li>Manchester City có 89 điểm (28 * 3 + 5 * 1) và hiệu số bàn thắng là 61 (94 - 33).</li>
        <li>Arsenal có 84 điểm (26 * 3 + 6 * 1) và hiệu số bàn thắng là 45 (88 - 43).</li>
        <li>Manchester United có 75 điểm (23 * 3 + 6 * 1) và hiệu số bàn thắng là 15 (58 - 43).</li>
        <li>Newcastle có 71 điểm (19 * 3 + 14 * 1) và hiệu số bàn thắng là 35 (68 - 33).</li>
        <li>Liverpool có 67 điểm (19 * 3 + 10 * 1) và hiệu số bàn thắng là 28 (75 - 47).</li>
    </ul>
    </li>
    <li>Các đội được xếp hạng trước tiên theo điểm số, sau đó theo hiệu số bàn thắng, và cuối cùng theo tên đội.</li>
    <li>Kết quả được sắp xếp theo season_id tăng dần, sau đó theo thứ hạng tăng dần, và cuối cùng theo team_name tăng dần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi mùa giải được xếp hạng theo điểm số, hiệu số bàn thắng, sau đó theo tên đội. Điểm số và hiệu số bàn thắng được suy ra từ số trận thắng, hòa và số bàn thắng.
>
> Vị trí trong một mùa giải là các số liên tiếp, không có khoảng cách thứ hạng, nên chỉ cần đánh số dòng theo từng nhóm.
>
> Sau khi sắp xếp theo $\textit{season\_id}$, điểm số, hiệu số bàn thắng và tên đội, $\textit{cumcount}+1$ chính là vị trí.

<!-- thinking:end -->

Ta có thể sử dụng hàm cửa sổ `RANK()` để xếp hạng các đội bằng cách nhóm theo mùa giải và sắp xếp theo điểm số, hiệu số bàn thắng và tên đội.

Cuối cùng, ta chỉ cần sắp xếp theo `season_id`, `position` và `team_name`.

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    season_id,
    team_id,
    team_name,
    wins * 3 + draws points,
    goals_for - goals_against goal_difference,
    RANK() OVER (
        PARTITION BY season_id
        ORDER BY wins * 3 + draws DESC, goals_for - goals_against DESC, team_name
    ) position
FROM SeasonStats
ORDER BY 1, 6, 3;
```

#### Pandas

```python
import pandas as pd


def process_team_standings(season_stats: pd.DataFrame) -> pd.DataFrame:
    season_stats["points"] = season_stats["wins"] * 3 + season_stats["draws"]
    season_stats["goal_difference"] = (
        season_stats["goals_for"] - season_stats["goals_against"]
    )

    season_stats = season_stats.sort_values(
        ["season_id", "points", "goal_difference", "team_name"],
        ascending=[True, False, False, True],
    )

    season_stats["position"] = season_stats.groupby("season_id").cumcount() + 1

    return season_stats[
        ["season_id", "team_id", "team_name", "points", "goal_difference", "position"]
    ]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
