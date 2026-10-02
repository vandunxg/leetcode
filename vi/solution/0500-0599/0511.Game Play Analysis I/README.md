---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [511. Game Play Analysis I](https://leetcode.com/problems/game-play-analysis-i)

[中文文档](/solution/0500-0599/0511.Game%20Play%20Analysis%20I/README.md)

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
Mỗi hàng ghi lại một người chơi đã đăng nhập và chơi một số ván (có thể là 0) trước khi đăng xuất vào một ngày nào đó bằng một thiết bị.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm <strong>ngày đăng nhập đầu tiên</strong> của mỗi người chơi.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

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
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |
+-----------+-----------+------------+--------------+
<strong>Đầu ra:</strong> 
+-----------+-------------+
| player_id | first_login |
+-----------+-------------+
| 1         | 2016-03-01  |
| 2         | 2017-06-25  |
| 3         | 2016-03-02  |
+-----------+-------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: GROUP BY + hàm MIN

<!-- thinking:start -->

> **Tư duy**
>
> Một người chơi có thể xuất hiện vào nhiều ngày; ta cần lấy `event_date` sớm nhất của từng người chơi. Có thể duyệt tuyến tính, nhưng cách biểu diễn theo mô hình quan hệ là phép tổng hợp theo nhóm.
>
> Dùng `GROUP BY player_id` cùng `MIN(event_date)` (hoặc phép lấy giá trị nhỏ nhất tương đương của `groupby`) để tìm lần đăng nhập đầu tiên. Chỉ cần một phép tổng hợp, không cần self-join.

<!-- thinking:end -->

Ta có thể dùng `GROUP BY` để nhóm theo `player_id`, sau đó lấy `event_date` nhỏ nhất trong mỗi nhóm làm ngày người chơi đăng nhập vào nền tảng lần đầu.

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def game_analysis(activity: pd.DataFrame) -> pd.DataFrame:
    return (
        activity.groupby("player_id")
        .agg(first_login=("event_date", "min"))
        .reset_index()
    )
```

#### MySQL

```sql
# Write your MySQL query statement below
SELECT player_id, MIN(event_date) AS first_login
FROM Activity
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
