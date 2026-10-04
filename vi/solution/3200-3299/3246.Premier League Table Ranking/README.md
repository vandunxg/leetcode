---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3246. Premier League Table Ranking 🔒](https://leetcode.com/problems/premier-league-table-ranking)

[中文文档](/solution/3200-3299/3246.Premier%20League%20Table%20Ranking/README.md)

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

<p>Viết lời giải để tính <strong>điểm</strong> và <strong>thứ hạng</strong> cho mỗi đội trong giải đấu. Điểm được tính như sau:</p>

<ul>
    <li><code>3</code> điểm cho một trận <strong>thắng</strong></li>
    <li><code>1</code> điểm cho một trận <strong>hòa</strong></li>
    <li><code>0</code> điểm cho một trận <strong>thua</strong></li>
</ul>

<p><strong>Lưu ý:</strong>&nbsp;Các đội có cùng số điểm phải được xếp cùng thứ hạng.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>points</code>&nbsp;<em>theo thứ tự <strong>giảm dần</strong>,<strong>&nbsp;</strong>sau đó theo</em> <code>team_name</code> <em>theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả truy vấn như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>TeamStats</code>:</p>

<pre class="example-io">
+---------+-----------------+----------------+------+-------+--------+
| team_id | team_name       | matches_played | wins | draws | losses |
+---------+-----------------+----------------+------+-------+--------+
| 1       | Manchester City | 10             | 6    | 2     | 2      |
| 2       | Liverpool       | 10             | 6    | 2     | 2      |
| 3       | Chelsea         | 10             | 5    | 3     | 2      |
| 4       | Arsenal         | 10             | 4    | 4     | 2      |
| 5       | Tottenham       | 10             | 3    | 5     | 2      |
+---------+-----------------+----------------+------+-------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+-----------------+--------+----------+
| team_id | team_name       | points | position |
+---------+-----------------+--------+----------+
| 2       | Liverpool       | 20     | 1        |
| 1       | Manchester City | 20     | 1        |
| 3       | Chelsea         | 18     | 3        |
| 4       | Arsenal         | 16     | 4        |
| 5       | Tottenham       | 14     | 5        |
+---------+-----------------+--------+----------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Manchester City và Liverpool đều có 20 điểm (6 trận thắng * 3 điểm + 2 trận hòa * 1 điểm), nên cùng xếp ở vị trí 1.</li>
    <li>Chelsea có 18 điểm (5 trận thắng * 3 điểm + 3 trận hòa * 1 điểm) và xếp ở vị trí thứ 3.</li>
    <li>Arsenal có 16 điểm (4 trận thắng * 3 điểm + 4 trận hòa * 1 điểm) và xếp ở vị trí thứ 4.</li>
    <li>Tottenham có 14 điểm (3 trận thắng * 3 điểm + 5 trận hòa * 1 điểm) và xếp ở vị trí thứ 5.</li>
</ul>

<p>Bảng kết quả được sắp xếp theo points theo thứ tự giảm dần, sau đó theo team_name theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Điểm là $3\times\textit{wins}+\textit{draws}$; thứ hạng được xác định theo điểm giảm dần và các đội hòa điểm cùng xếp một vị trí. Có thể tự sắp xếp rồi duyệt để xử lý các trường hợp bằng điểm; window function thực hiện việc này trong một bước.
>
> Sau khi tính `points`, `rank(method='min')` cho ra vị trí, rồi sắp xếp theo điểm và tên đội. Min-rank phù hợp với quy ước xếp hạng thể thao.

<!-- thinking:end -->

Có thể dùng hàm cửa sổ `RANK()` để tính thứ hạng của các đội, sau đó sắp xếp theo điểm và tên đội.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    team_id,
    team_name,
    wins * 3 + draws points,
    RANK() OVER (ORDER BY (wins * 3 + draws) DESC) position
FROM TeamStats
ORDER BY 3 DESC, 2;
```

#### Pandas

```python
import pandas as pd


def calculate_team_standings(team_stats: pd.DataFrame) -> pd.DataFrame:
    team_stats["points"] = team_stats["wins"] * 3 + team_stats["draws"]
    team_stats["position"] = team_stats["points"].rank(method="min", ascending=False)
    team_stats = team_stats.sort_values(
        by=["points", "team_name"], ascending=[False, True]
    )
    return team_stats[["team_id", "team_name", "points", "position"]]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
