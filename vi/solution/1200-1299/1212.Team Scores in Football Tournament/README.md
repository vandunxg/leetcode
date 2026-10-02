---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1212. Team Scores in Football Tournament 🔒](https://leetcode.com/problems/team-scores-in-football-tournament)

[中文文档](/solution/1200-1299/1212.Team%20Scores%20in%20Football%20Tournament/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Teams</code></p>

<pre>
+---------------+----------+
| Column Name   | Type     |
+---------------+----------+
| team_id       | int      |
| team_name     | varchar  |
+---------------+----------+
`team_id` là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng trong bảng này biểu diễn một đội bóng.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Matches</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| match_id      | int     |
| host_team     | int     |
| guest_team    | int     | 
| host_goals    | int     |
| guest_goals   | int     |
+---------------+---------+
`match_id` là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng ghi lại một trận đấu đã kết thúc giữa hai đội khác nhau. 
Các đội chủ nhà `host_team` và đội khách `guest_team` được biểu diễn bằng ID của họ trong bảng Teams (`team_id`); số bàn thắng tương ứng được lưu trong `host_goals` và `guest_goals`.
</pre>

<p>&nbsp;</p>
Hãy tính điểm của tất cả các đội sau khi mọi trận đấu kết thúc. Điểm được tính như sau:

<ul>
	<li>Đội thắng trận (ghi nhiều bàn hơn đối thủ) nhận <strong>ba điểm</strong>.</li>
	<li>Hai đội hòa (ghi số bàn bằng nhau) thì mỗi đội nhận <strong>một điểm</strong>.</li>
	<li>Đội thua trận (ghi ít bàn hơn đối thủ) không nhận điểm.</li>
</ul>

<p>Hãy viết lời giải chọn <code>team_id</code>, <code>team_name</code> và <code>num_points</code> của mỗi đội trong giải đấu sau tất cả các trận đã nêu.</p>

<p>Trả về bảng kết quả, sắp xếp theo <code>num_points</code> <strong>giảm dần</strong>. Nếu bằng điểm, sắp xếp các hàng theo <code>team_id</code> <strong>tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Teams table:
+-----------+--------------+
| team_id   | team_name    |
+-----------+--------------+
| 10        | Leetcode FC  |
| 20        | NewYork FC   |
| 30        | Atlanta FC   |
| 40        | Chicago FC   |
| 50        | Toronto FC   |
+-----------+--------------+
Matches table:
+------------+--------------+---------------+-------------+--------------+
| match_id   | host_team    | guest_team    | host_goals  | guest_goals  |
+------------+--------------+---------------+-------------+--------------+
| 1          | 10           | 20            | 3           | 0            |
| 2          | 30           | 10            | 2           | 2            |
| 3          | 10           | 50            | 5           | 1            |
| 4          | 20           | 30            | 1           | 0            |
| 5          | 50           | 30            | 1           | 0            |
+------------+--------------+---------------+-------------+--------------+
<strong>Đầu ra:</strong> 
+------------+--------------+---------------+
| team_id    | team_name    | num_points    |
+------------+--------------+---------------+
| 10         | Leetcode FC  | 7             |
| 20         | NewYork FC   | 3             |
| 50         | Toronto FC   | 3             |
| 30         | Atlanta FC   | 1             |
| 40         | Chicago FC   | 0             |
+------------+--------------+---------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: LEFT JOIN + GROUP BY + biểu thức CASE

<!-- thinking:start -->

> **Tư duy**
>
> Một đội có thể không thi đấu trận nào; `INNER JOIN` sẽ làm mất hàng có điểm 0 của đội đó. Dùng `LEFT JOIN` từ $Teams$ sang $Matches$ với điều kiện đội là chủ nhà hoặc đội khách sẽ giữ lại các đội này.
>
> Điểm chỉ phụ thuộc vào việc đội đó là chủ nhà hay đội khách và so sánh số bàn thắng: thắng được $3$ điểm, hòa được $1$ điểm, còn lại được $0$ điểm. $CASE$ tính điểm cho từng hàng; sau đó ta tính tổng theo $team\_id$.
>
> Ta sắp xếp điểm giảm dần và $team\_id$ tăng dần. `LEFT JOIN` giữ đủ danh sách đội; $CASE$ chuyển quy tắc thắng/hòa/thua thành giá trị có thể cộng gộp.

<!-- thinking:end -->

Ta dùng `LEFT JOIN` để nối bảng `Teams` với bảng `Matches` theo điều kiện `team_id = host_team OR team_id = guest_team`, qua đó lấy thông tin các trận đấu của từng đội.

Tiếp theo, ta nhóm theo `team_id` và dùng biểu thức `CASE` để tính điểm cho mỗi đội theo các quy tắc sau:

- Nếu đội là chủ nhà và ghi nhiều bàn hơn đội khách, cộng $3$ điểm vào tổng điểm của đội.
- Nếu đội là đội khách và ghi nhiều bàn hơn đội chủ nhà, cộng $3$ điểm vào tổng điểm của đội.
- Nếu đội chủ nhà và đội khách ghi cùng số bàn, cộng $1$ điểm vào tổng điểm của đội.

Cuối cùng, ta sắp xếp kết quả theo điểm giảm dần; nếu bằng điểm thì sắp xếp theo `team_id` tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    team_id,
    team_name,
    SUM(
        CASE
            WHEN team_id = host_team
            AND host_goals > guest_goals THEN 3
            WHEN team_id = guest_team
            AND guest_goals > host_goals THEN 3
            WHEN host_goals = guest_goals THEN 1
            ELSE 0
        END
    ) AS num_points
FROM
    Teams
    LEFT JOIN Matches ON team_id = host_team OR team_id = guest_team
GROUP BY 1
ORDER BY 3 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
