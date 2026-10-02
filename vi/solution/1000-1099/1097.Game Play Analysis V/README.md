---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1097. Game Play Analysis V 🔒](https://leetcode.com/problems/game-play-analysis-v)

[中文文档](/solution/1000-1099/1097.Game%20Play%20Analysis%20V/README.md)

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
(player_id, event_date) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng này ghi lại hoạt động của người chơi trong một số trò chơi.
Mỗi hàng ghi lại một lần người chơi đăng nhập bằng một thiết bị, chơi một số ván (có thể là 0) rồi đăng xuất vào một ngày nào đó.
</pre>

<p>&nbsp;</p>

<p><strong>Ngày cài đặt</strong> của một người chơi là ngày đầu tiên người đó đăng nhập.</p>

<p><strong>Tỷ lệ giữ chân ngày thứ nhất</strong> của ngày <code>x</code> là số người chơi có <strong>ngày cài đặt</strong> là <code>x</code> và đăng nhập lại vào ngày ngay sau <code>x</code>, chia cho tổng số người chơi có ngày cài đặt là <code>x</code>; kết quả được làm tròn đến <code>2</code> chữ số thập phân.</p>

<p>Hãy viết lời giải để trả về số người chơi cài đặt trò chơi vào mỗi ngày cài đặt và <strong>tỷ lệ giữ chân ngày thứ nhất</strong> tương ứng.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Activity table:
+-----------+-----------+------------+--------------+
| player_id | device_id | event_date | games_played |
+-----------+-----------+------------+--------------+
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-03-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-01 | 0            |
| 3         | 4         | 2016-07-03 | 5            |
+-----------+-----------+------------+--------------+
<strong>Output:</strong> 
+------------+----------+----------------+
| install_dt | installs | Day1_retention |
+------------+----------+----------------+
| 2016-03-01 | 2        | 0.50           |
| 2017-06-25 | 1        | 0.00           |
+------------+----------+----------------+
<strong>Giải thích:</strong> 
Người chơi 1 và 3 cài đặt trò chơi vào ngày 2016-03-01, nhưng chỉ người chơi 1 đăng nhập lại vào ngày 2016-03-02. Vì vậy, tỷ lệ giữ chân ngày thứ nhất của ngày 2016-03-01 là 1 / 2 = 0.50
Người chơi 2 cài đặt trò chơi vào ngày 2017-06-25 nhưng không đăng nhập lại vào ngày 2017-06-26. Vì vậy, tỷ lệ giữ chân ngày thứ nhất của ngày 2017-06-25 là 0 / 1 = 0.00
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ngày cài đặt của mỗi người chơi là `event_date` sớm nhất của họ. Tỷ lệ giữ chân ngày thứ nhất là tỷ lệ người chơi trong nhóm đó có đăng nhập vào ngày hôm sau, tính theo từng ngày cài đặt.
>
> `MIN(event_date) OVER (PARTITION BY player_id)` gắn ngày cài đặt vào từng hàng. Query bên ngoài đếm số người chơi khác nhau theo ngày cài đặt rồi chia `SUM(datediff = 1)` cho số lượng đó.
>
> `ROUND(..., 2)` giữ lại hai chữ số thập phân.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            player_id,
            event_date,
            MIN(event_date) OVER (PARTITION BY player_id) AS install_dt
        FROM Activity
    )
SELECT
    install_dt,
    COUNT(DISTINCT player_id) AS installs,
    ROUND(
        SUM(DATEDIFF(event_date, install_dt) = 1) / COUNT(DISTINCT player_id),
        2
    ) AS day1_retention
FROM T
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
