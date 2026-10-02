---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [534. Game Play Analysis III 🔒](https://leetcode.com/problems/game-play-analysis-iii)

[中文文档](/solution/0500-0599/0534.Game%20Play%20Analysis%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Activity</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
(player_id, event_date) là khóa chính (cột có giá trị duy nhất) của bảng này.
Bảng này ghi lại hoạt động chơi game của người chơi.
Mỗi hàng ghi lại việc một người chơi đăng nhập bằng một thiết bị nào đó và chơi một số game (có thể là 0) trước khi đăng xuất vào một ngày cụ thể.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo số game mà mỗi player đã chơi <strong>tính đến thời điểm đó</strong>, cho từng player và từng ngày. Nói cách khác, đây là tổng số game player đã chơi cho đến ngày tương ứng. Xem ví dụ để hiểu rõ hơn.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Activity:
+-----------+-----------+------------+--------------+
| player_id | device_id | event_date | games_played |
+-----------+-----------+------------+--------------+
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-05-02 | 6            |
| 1         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |
+-----------+-----------+------------+--------------+
<strong>Đầu ra:</strong> 
+-----------+------------+---------------------+
| player_id | event_date | games_played_so_far |
+-----------+------------+---------------------+
| 1         | 2016-03-01 | 5                   |
| 1         | 2016-05-02 | 11                  |
| 1         | 2017-06-25 | 12                  |
| 3         | 2016-03-02 | 0                   |
| 3         | 2018-07-03 | 5                   |
+-----------+------------+---------------------+
<strong>Giải thích:</strong> 
Với player có id 1, đến ngày 2016-05-02 đã chơi 5 + 6 = 11 game, còn đến ngày 2017-06-25 đã chơi 5 + 6 + 1 = 12 game.
Với player có id 3, đến ngày 2018-07-03 đã chơi 0 + 5 = 5 game.
Lưu ý, với mỗi player, ta chỉ xét những ngày người đó đăng nhập.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính tổng lũy kế số game của từng player theo ngày. Có thể dùng self-join, nhưng window function thực hiện việc này chỉ trong một lượt quét.
>
> `SUM(games_played) OVER (PARTITION BY player_id ORDER BY event_date)` cộng dồn theo thứ tự ngày trong phạm vi từng player. Không cần thêm bước group rồi join.

<!-- thinking:end -->

Ta có thể dùng window function `SUM() OVER()` để phân vùng theo `player_id`, sắp xếp theo `event_date`, rồi tính tổng số game mỗi người chơi đã chơi tính đến ngày hiện tại.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    player_id,
    event_date,
    SUM(games_played) OVER (
        PARTITION BY player_id
        ORDER BY event_date
    ) AS games_played_so_far
FROM Activity;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Self-Join + Group By

<!-- thinking:start -->

> **Tư duy**
>
> Nếu không dùng window function, ta self-join ngày của mỗi player với tất cả ngày trước đó hoặc cùng ngày của chính player đó, rồi `GROUP BY` player và ngày.
>
> Điều kiện `t1.event_date >= t2.event_date` giữ lại các hàng thuộc ngày trước đó và ngày hiện tại. Tổng thu được giống phần lũy kế của window function, nhưng phải xử lý nhiều cặp hàng hơn.

<!-- thinking:end -->

Ta cũng có thể self-join bảng `Activity` với chính nó theo điều kiện `t1.player_id = t2.player_id AND t1.event_date >= t2.event_date`, sau đó nhóm theo `t1.player_id` và `t1.event_date`, rồi tính tổng lũy kế của `t2.games_played`. Kết quả là tổng số game mỗi người chơi đã chơi tính đến ngày hiện tại.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    t1.player_id,
    t1.event_date,
    SUM(t2.games_played) AS games_played_so_far
FROM
    Activity AS t1,
    Activity AS t2
WHERE t1.player_id = t2.player_id AND t1.event_date >= t2.event_date
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 viết cùng điều kiện bằng comma join. `CROSS JOIN ... ON` chỉ chuyển điều kiện lọc vào mệnh đề join.
>
> Đây chỉ là khác biệt về cách viết; các tổng sau khi nhóm giống với lời giải 2.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    t1.player_id,
    t1.event_date,
    SUM(t2.games_played) AS games_played_so_far
FROM
    Activity AS t1
    CROSS JOIN Activity AS t2 ON t1.player_id = t2.player_id AND t1.event_date >= t2.event_date
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
