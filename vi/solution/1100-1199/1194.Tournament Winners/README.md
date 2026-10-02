---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1194. Tournament Winners 🔒](https://leetcode.com/problems/tournament-winners)

[中文文档](/solution/1100-1199/1194.Tournament%20Winners/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Players</code></p>

<pre>
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| player_id   | int   |
| group_id    | int   |
+-------------+-------+
player_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết nhóm của một người chơi.
</pre>

<p>Bảng: <code>Matches</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| match_id      | int     |
| first_player  | int     |
| second_player | int     | 
| first_score   | int     |
| second_score  | int     |
+---------------+---------+
match_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng ghi lại một trận đấu; first_player và second_player chứa player_id của hai người chơi trong trận.
first_score và second_score lần lượt chứa số điểm của first_player và second_player.
Bạn có thể giả định rằng những người chơi trong mỗi trận đấu đều thuộc cùng một nhóm.
</pre>

<p>&nbsp;</p>

<p>Người chiến thắng trong mỗi nhóm là người chơi có tổng điểm cao nhất trong nhóm đó. Nếu hòa điểm, người chơi có <code>player_id</code> <strong>nhỏ nhất</strong> sẽ thắng.</p>

<p>Viết lời giải để tìm người chiến thắng trong mỗi nhóm.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Players:
+-----------+------------+
| player_id | group_id   |
+-----------+------------+
| 15        | 1          |
| 25        | 1          |
| 30        | 1          |
| 45        | 1          |
| 10        | 2          |
| 35        | 2          |
| 50        | 2          |
| 20        | 3          |
| 40        | 3          |
+-----------+------------+
Bảng Matches:
+------------+--------------+---------------+-------------+--------------+
| match_id   | first_player | second_player | first_score | second_score |
+------------+--------------+---------------+-------------+--------------+
| 1          | 15           | 45            | 3           | 0            |
| 2          | 30           | 25            | 1           | 2            |
| 3          | 30           | 15            | 2           | 0            |
| 4          | 40           | 20            | 5           | 2            |
| 5          | 35           | 50            | 1           | 1            |
+------------+--------------+---------------+-------------+--------------+
<strong>Đầu ra:</strong> 
+-----------+------------+
| group_id  | player_id  |
+-----------+------------+ 
| 1         | 15         |
| 2         | 35         |
| 3         | 40         |
+-----------+------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trong mỗi nhóm, chọn người chơi có tổng điểm cao nhất; nếu hòa thì ưu tiên `player_id` nhỏ hơn. Chuyển dữ liệu từ hai phía của mỗi trận thành các hàng điểm, tính tổng theo người chơi, rồi dùng `RANK` theo `(scores DESC, player_id)` trong từng nhóm và giữ hàng có $rk=1$. `UNION ALL` giúp tính điểm của cả người chơi ở phía first và second.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    s AS (
        SELECT first_player AS player_id, first_score AS score, group_id
        FROM
            Matches AS m
            JOIN Players AS p ON m.first_player = p.player_id
        UNION ALL
        SELECT second_player AS player_id, second_score AS score, group_id
        FROM
            Matches AS m
            JOIN Players AS p ON m.second_player = p.player_id
    ),
    t AS (
        SELECT group_id, player_id, SUM(score) AS scores
        FROM s
        GROUP BY player_id
    ),
    p AS (
        SELECT
            group_id,
            player_id,
            RANK() OVER (
                PARTITION BY group_id
                ORDER BY scores DESC, player_id
            ) AS rk
        FROM t
    )
SELECT group_id, player_id
FROM p
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
