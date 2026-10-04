---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3060. User Activities within Time Bounds 🔒](https://leetcode.com/problems/user-activities-within-time-bounds)

[中文文档](/solution/3000-3099/3060.User%20Activities%20within%20Time%20Bounds/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Sessions</code></p>

<pre>
+---------------+----------+
| Column Name   | Type     |
+---------------+----------+
| user_id       | int      |
| session_start | datetime |
| session_end   | datetime |
| session_id    | int      |
| session_type  | enum     |
+---------------+----------+
session_id là cột chứa các giá trị duy nhất của bảng này.
session_type là kiểu ENUM (phân loại) gồm (Viewer, Streamer).
Bảng này chứa user id, thời gian bắt đầu session, thời gian kết thúc session, session id và loại session.
</pre>

<p>Hãy viết lời giải để tìm những <strong>người dùng</strong> đã có <strong>ít nhất hai</strong> <strong>session</strong> cùng <strong>loại</strong> (hoặc &#39;<strong>Viewer</strong>&#39; hoặc &#39;<strong>Streamer</strong>&#39;) với khoảng cách <strong>tối đa</strong> <code>12</code> giờ <strong>giữa</strong> các session.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo </em><code>user_id</code><em> theo <b>thứ tự tăng dần</b>.</em></p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Sessions:
+---------+---------------------+---------------------+------------+--------------+
| user_id | session_start       | session_end         | session_id | session_type |
+---------+---------------------+---------------------+------------+--------------+
| 101     | 2023-11-01 08:00:00 | 2023-11-01 09:00:00 | 1          | Viewer       |
| 101     | 2023-11-01 10:00:00 | 2023-11-01 11:00:00 | 2          | Streamer     |
| 102     | 2023-11-01 13:00:00 | 2023-11-01 14:00:00 | 3          | Viewer       |
| 102     | 2023-11-01 15:00:00 | 2023-11-01 16:00:00 | 4          | Viewer       |
| 101     | 2023-11-02 09:00:00 | 2023-11-02 10:00:00 | 5          | Viewer       |
| 102     | 2023-11-02 12:00:00 | 2023-11-02 13:00:00 | 6          | Streamer     |
| 101     | 2023-11-02 13:00:00 | 2023-11-02 14:00:00 | 7          | Streamer     |
| 102     | 2023-11-02 16:00:00 | 2023-11-02 17:00:00 | 8          | Viewer       |
| 103     | 2023-11-01 08:00:00 | 2023-11-01 09:00:00 | 9          | Viewer       |
| 103     | 2023-11-02 20:00:00 | 2023-11-02 23:00:00 | 10         | Viewer       |
| 103     | 2023-11-03 09:00:00 | 2023-11-03 10:00:00 | 11         | Viewer       |
+---------+---------------------+---------------------+------------+--------------+
<strong>Đầu ra:</strong>
+---------+
| user_id |
+---------+
| 102     |
| 103     |
+---------+
<strong>Giải thích:</strong>
- User ID 101 không được đưa vào kết quả cuối cùng vì không có hai session nào cùng loại.
- User ID 102 được đưa vào kết quả vì có hai session loại Viewer với session ID lần lượt là 3 và 4, đồng thời khoảng cách thời gian giữa chúng nhỏ hơn 12 giờ.
- User 103 đã tham gia hai session loại Viewer với khoảng cách giữa chúng nhỏ hơn 12 giờ, được xác định bởi session ID 10 và 11. Vì vậy, user 103 được đưa vào kết quả.
Bảng kết quả được sắp xếp theo user_id theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ + Hàm thời gian

<!-- thinking:start -->

> **Tư duy**
>
> Một người dùng đạt yêu cầu khi có hai session cùng loại, trong đó thời điểm bắt đầu của session sau nằm trong vòng $12$ giờ kể từ thời điểm kết thúc của session trước.
>
> Sau khi sắp xếp theo user và loại, thời điểm kết thúc trước đó được lấy bằng phép shift, rồi so sánh với thời điểm bắt đầu hiện tại.
>
> Phép shift theo nhóm tạo ra $\textit{prev\_session\_end}$; sau đó lọc theo timedelta $12$ giờ và loại bỏ các user trùng nhau.

<!-- thinking:end -->

Trước tiên, chúng ta sử dụng hàm cửa sổ `LAG` để tìm thời điểm kết thúc của session trước đó cùng loại với mỗi user, được ký hiệu là `prev_session_end`. Sau đó, chúng ta sử dụng hàm `TIMESTAMPDIFF` để tính khoảng thời gian giữa thời điểm bắt đầu của session hiện tại và thời điểm kết thúc của session trước đó. Nếu khoảng thời gian này nhỏ hơn hoặc bằng 12 giờ, user đó thỏa mãn yêu cầu của bài toán.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            user_id,
            session_start,
            LAG(session_end) OVER (
                PARTITION BY user_id, session_type
                ORDER BY session_end
            ) AS prev_session_end
        FROM Sessions
    )
SELECT DISTINCT
    user_id
FROM T
WHERE TIMESTAMPDIFF(HOUR, prev_session_end, session_start) <= 12;
```

#### Python3

```python
import pandas as pd


def user_activities(sessions: pd.DataFrame) -> pd.DataFrame:
    sessions = sessions.sort_values(by=["user_id", "session_start"])
    sessions["prev_session_end"] = sessions.groupby(["user_id", "session_type"])[
        "session_end"
    ].shift(1)
    sessions_filtered = sessions[
        sessions["session_start"] - sessions["prev_session_end"]
        <= pd.Timedelta(hours=12)
    ]
    return pd.DataFrame({"user_id": sessions_filtered["user_id"].unique()})
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
