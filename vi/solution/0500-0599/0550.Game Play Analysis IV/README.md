---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv)

[中文文档](/solution/0500-0599/0550.Game%20Play%20Analysis%20IV/README.md)

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
Bảng này ghi lại hoạt động chơi game của người chơi.
Mỗi hàng ghi lại việc một người chơi đăng nhập bằng một thiết bị nào đó và chơi một số game (có thể là 0) trước khi đăng xuất vào một ngày cụ thể.
</pre>

<p> </p>

<p>Hãy viết lời giải để tính <strong>tỷ lệ</strong> người chơi đăng nhập lại vào ngày sau ngày đăng nhập đầu tiên, <strong>làm tròn đến 2 chữ số thập phân</strong>. Nói cách khác, hãy đếm số người chơi đăng nhập vào đúng ngày kế tiếp sau lần đăng nhập đầu tiên, rồi chia cho tổng số người chơi.</p>

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
| 1         | 2         | 2016-03-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |
+-----------+-----------+------------+--------------+
<strong>Đầu ra:</strong> 
+-----------+
| fraction  |
+-----------+
| 0.33      |
+-----------+
<strong>Giải thích:</strong> 
Chỉ người chơi có id 1 đăng nhập lại vào ngày sau ngày đầu tiên họ đăng nhập, nên kết quả là 1/3 = 0.33
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Grouping và giá trị nhỏ nhất + Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Retention là tỷ lệ người chơi đăng nhập vào ngày sau lần đăng nhập đầu tiên. Ta cần tìm ngày đầu tiên của mỗi người chơi, rồi kiểm tra xem có dữ liệu đăng nhập vào ngày kế tiếp hay không.
>
> Một subquery lấy `MIN(event_date)` của từng người chơi rồi left join với bảng gốc theo cùng player và chênh lệch ngày là $1$. Nếu không có lần đăng nhập vào ngày kế tiếp thì giá trị vẫn là `NULL`; lấy trung bình của `event_date IS NOT NULL` sẽ cho ra tỷ lệ cần tìm.

<!-- thinking:end -->

Trước tiên, ta tìm ngày đăng nhập đầu tiên của mỗi người chơi, sau đó left join với bảng gốc theo điều kiện player ID giống nhau và chênh lệch ngày là $-1$, tức người chơi có đăng nhập vào ngày kế tiếp. Cuối cùng, ta tính tỷ lệ các giá trị không null trong số những người chơi đăng nhập vào ngày thứ hai.

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def gameplay_analysis(activity: pd.DataFrame) -> pd.DataFrame:
    activity["first"] = activity.groupby("player_id").event_date.transform(min)
    activity_2nd_day = activity[
        activity["first"] + pd.DateOffset(1) == activity["event_date"]
    ]

    return pd.DataFrame(
        {"fraction": [round(len(activity_2nd_day) / activity.player_id.nunique(), 2)]}
    )
```

#### MySQL

```sql
# Write your MySQL query statement below
SELECT ROUND(AVG(b.event_date IS NOT NULL), 2) AS fraction
FROM
    (
        SELECT player_id, MIN(event_date) AS event_date
        FROM Activity
        GROUP BY 1
    ) AS a
    LEFT JOIN Activity AS b
        ON a.player_id = b.player_id AND DATEDIFF(a.event_date, b.event_date) = -1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 gom nhóm rồi join. Window function có thể lấy ngày đăng nhập kế tiếp và xếp hạng các ngày chỉ trong một lượt.
>
> `LEAD` trả về ngày đăng nhập kế tiếp, còn `RANK` đánh dấu lần đăng nhập đầu tiên. Giữ các hàng có rank $1$ rồi tính tỷ lệ ngày kế tiếp cách đúng một ngày. Cách này cho cùng kết quả mà không cần self-join tường minh.

<!-- thinking:end -->

Ta có thể dùng window function `LEAD` để lấy ngày đăng nhập kế tiếp của mỗi người chơi. Nếu ngày đó cách ngày đăng nhập hiện tại đúng một ngày, người chơi đã đăng nhập vào ngày kế tiếp; ta dùng trường $st$ để ghi nhận điều này. Sau đó, dùng window function `RANK` để xếp hạng các lần đăng nhập của từng người chơi theo ngày tăng dần. Cuối cùng, tính tỷ lệ giá trị $st$ khác null trong số những người chơi có thứ hạng $1$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            player_id,
            DATEDIFF(
                LEAD(event_date) OVER (
                    PARTITION BY player_id
                    ORDER BY event_date
                ),
                event_date
            ) = 1 AS st,
            RANK() OVER (
                PARTITION BY player_id
                ORDER BY event_date
            ) AS rk
        FROM Activity
    )
SELECT ROUND(COUNT(IF(st = 1, player_id, NULL)) / COUNT(DISTINCT player_id), 2) AS fraction
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
