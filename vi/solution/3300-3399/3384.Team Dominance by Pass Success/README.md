---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3384. Team Dominance by Pass Success 🔒](https://leetcode.com/problems/team-dominance-by-pass-success)

[中文文档](/solution/3300-3399/3384.Team%20Dominance%20by%20Pass%20Success/README.md)

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
(pass_from, time_stamp) là khóa chính của bảng này.
pass_from là khóa ngoại tham chiếu đến player_id trong bảng Teams.
Mỗi dòng biểu diễn một đường chuyền được thực hiện trong trận đấu, time_stamp là thời điểm tính bằng phút (00:00-90:00) khi đường chuyền được thực hiện,
pass_to là player_id của cầu thủ nhận đường chuyền.

</pre>

<p>Hãy viết lời giải để tính <strong>điểm áp đảo</strong> cho mỗi đội trong<strong> cả hai hiệp của trận đấu</strong>. Các quy tắc như sau:</p>

<ul>
    <li>Một trận đấu được chia thành hai hiệp: <strong>hiệp một</strong> (<code>00:00</code>-<code><font face="monospace">45:00</font></code>&nbsp;phút) và <strong>hiệp hai </strong>(<code>45:01</code>-<code>90:00</code> phút)</li>
    <li>Điểm áp đảo được tính dựa trên các đường chuyền thành công và bị cắt:
    <ul>
        <li>Khi pass_to là cầu thủ của <strong>cùng đội</strong>: +<code>1</code> điểm</li>
        <li>Khi pass_to là cầu thủ của <strong>đội đối phương</strong> (bị cắt): <code>-1</code> điểm</li>
    </ul>
    </li>
    <li>Điểm áp đảo cao hơn cho thấy hiệu suất chuyền bóng tốt hơn</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp </em><em>theo</em>&nbsp;<code>team_name</code> và&nbsp;<code>half_number</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Teams:</p>

<pre class="example-io">
+------------+-----------+
| player_id  | team_name |
+------------+-----------+
| 1          | Arsenal   |
| 2          | Arsenal   |
| 3          | Arsenal   |
| 4          | Chelsea   |
| 5          | Chelsea   |
| 6          | Chelsea   |
+------------+-----------+
</pre>

<p>Bảng Passes:</p>

<pre class="example-io">
+-----------+------------+---------+
| pass_from | time_stamp | pass_to |
+-----------+------------+---------+
| 1         | 00:15      | 2       |
| 2         | 00:45      | 3       |
| 3         | 01:15      | 1       |
| 4         | 00:30      | 1       |
| 2         | 46:00      | 3       |
| 3         | 46:15      | 4       |
| 1         | 46:45      | 2       |
| 5         | 46:30      | 6       |
+-----------+------------+---------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-----------+-------------+-----------+
| team_name | half_number | dominance |
+-----------+-------------+-----------+
| Arsenal   | 1           | 3         |
| Arsenal   | 2           | 1         |
| Chelsea   | 1           | -1        |
| Chelsea   | 2           | 1         |
+-----------+-------------+-----------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Hiệp một (00:00-45:00):</strong>

    <ul>
        <li>Các đường chuyền của Arsenal:
        <ul>
            <li>1 &rarr; 2 (00:15): Đường chuyền thành công (+1)</li>
            <li>2 &rarr; 3 (00:45): Đường chuyền thành công (+1)</li>
            <li>3 &rarr; 1 (01:15): Đường chuyền thành công (+1)</li>
        </ul>
        </li>
        <li>Các đường chuyền của Chelsea:
        <ul>
            <li>4 &rarr; 1 (00:30): Bị Arsenal cắt bóng (-1)</li>
        </ul>
        </li>
    </ul>
    </li>
    <li><strong>Hiệp hai (45:01-90:00):</strong>
    <ul>
        <li>Các đường chuyền của Arsenal:
        <ul>
            <li>2 &rarr; 3 (46:00): Đường chuyền thành công (+1)</li>
            <li>3 &rarr; 4 (46:15): Bị Chelsea cắt bóng (-1)</li>
            <li>1 &rarr; 2 (46:45): Đường chuyền thành công (+1)</li>
        </ul>
        </li>
        <li>Các đường chuyền của Chelsea:
        <ul>
            <li>5 &rarr; 6 (46:30): Đường chuyền thành công (+1)</li>
        </ul>
        </li>
    </ul>
    </li>
    <li>Kết quả được sắp xếp theo team_name rồi đến half_number</li>

</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + Group By và tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đường chuyền được tính $+1$ nếu trong cùng đội và $-1$ nếu giữa hai đội, sau đó được tổng hợp theo đội và hiệp. Các timestamp không vượt quá $45{:}00$ thuộc hiệp một.
>
> Join danh sách đường chuyền với bảng đội theo cả hai player id để so sánh tên hai đội.
>
> Group theo đội của người chuyền và hiệp, tính tổng dominance, rồi sắp xếp theo đội và hiệp.

<!-- thinking:end -->

Ta có thể sử dụng equi-join để tìm đội của cả người chuyền và người nhận cho mỗi đường chuyền. Sau đó, dựa trên timestamp, ta xác định đường chuyền diễn ra trong hiệp một hay hiệp hai. Bằng cách kiểm tra người chuyền và người nhận có thuộc cùng đội hay không, ta tính giá trị advantage cho mỗi đường chuyền. Cuối cùng, ta nhóm theo tên đội và số hiệp, rồi tính tổng các giá trị advantage để nhận được giá trị advantage của mỗi đội trong hiệp một và hiệp hai.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            t1.team_name,
            IF(time_stamp <= '45:00', 1, 2) half_number,
            IF(t1.team_name = t2.team_name, 1, -1) dominance
        FROM
            Passes p
            JOIN Teams t1 ON p.pass_from = t1.player_id
            JOIN Teams t2 ON p.pass_to = t2.player_id
    )
SELECT team_name, half_number, SUM(dominance) dominance
FROM T
GROUP BY 1, 2
ORDER BY 1, 2;
```

#### Pandas

```python
import pandas as pd


def calculate_team_dominance(teams: pd.DataFrame, passes: pd.DataFrame) -> pd.DataFrame:
    passes_with_teams = passes.merge(
        teams, left_on="pass_from", right_on="player_id", suffixes=("", "_team_from")
    ).merge(
        teams,
        left_on="pass_to",
        right_on="player_id",
        suffixes=("_team_from", "_team_to"),
    )
    passes_with_teams["half_number"] = passes_with_teams["time_stamp"].apply(
        lambda x: 1 if x <= "45:00" else 2
    )
    passes_with_teams["dominance"] = passes_with_teams.apply(
        lambda row: 1 if row["team_name_team_from"] == row["team_name_team_to"] else -1,
        axis=1,
    )
    result = (
        passes_with_teams.groupby(["team_name_team_from", "half_number"])["dominance"]
        .sum()
        .reset_index()
    )
    result.columns = ["team_name", "half_number", "dominance"]
    result = result.sort_values(by=["team_name", "half_number"])
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
