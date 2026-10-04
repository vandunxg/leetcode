---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3673. Find Zombie Sessions](https://leetcode.com/problems/find-zombie-sessions)

[中文文档](/solution/3600-3699/3673.Find%20Zombie%20Sessions/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>app_events</code></p>

<pre>
+------------------+----------+
| Column Name      | Type     |
+------------------+----------+
| event_id         | int      |
| user_id          | int      |
| event_timestamp  | datetime |
| event_type       | varchar  |
| session_id       | varchar  |
| event_value      | int      |
+------------------+----------+
event_id là định danh duy nhất của bảng này.
event_type có thể là app_open, click, scroll, purchase hoặc app_close.
session_id nhóm các event trong cùng một user session.
event_value biểu thị: với purchase - số tiền tính bằng đô la, với scroll - số pixel đã scroll, các trường hợp khác - NULL.
</pre>

<p>Viết lời giải để xác định <strong>zombie session</strong>, tức những session mà người dùng có vẻ đang hoạt động nhưng thể hiện các pattern hành vi bất thường. Một session được xem là <strong>zombie session</strong> nếu thỏa mãn TẤT CẢ các tiêu chí sau:</p>

<ul>
	<li>Thời lượng session <strong>lớn hơn</strong> <code>30</code> phút.</li>
	<li>Có <strong>ít nhất</strong> <code>5</code> event scroll.</li>
	<li><strong>Tỷ lệ click trên scroll</strong> nhỏ hơn <code>0.20</code> .</li>
	<li><strong>Không có purchase</strong> nào trong session.</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo</em>&nbsp;<code>scroll_count</code> <em>theo thứ tự <strong>giảm dần</strong>, sau đó theo</em> <code>session_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng app_events:</p>

<pre class="example-io">
+----------+---------+---------------------+------------+------------+-------------+
| event_id | user_id | event_timestamp     | event_type | session_id | event_value |
+----------+---------+---------------------+------------+------------+-------------+
| 1        | 201     | 2024-03-01 10:00:00 | app_open   | S001       | NULL        |
| 2        | 201     | 2024-03-01 10:05:00 | scroll     | S001       | 500         |
| 3        | 201     | 2024-03-01 10:10:00 | scroll     | S001       | 750         |
| 4        | 201     | 2024-03-01 10:15:00 | scroll     | S001       | 600         |
| 5        | 201     | 2024-03-01 10:20:00 | scroll     | S001       | 800         |
| 6        | 201     | 2024-03-01 10:25:00 | scroll     | S001       | 550         |
| 7        | 201     | 2024-03-01 10:30:00 | scroll     | S001       | 900         |
| 8        | 201     | 2024-03-01 10:35:00 | app_close  | S001       | NULL        |
| 9        | 202     | 2024-03-01 11:00:00 | app_open   | S002       | NULL        |
| 10       | 202     | 2024-03-01 11:02:00 | click      | S002       | NULL        |
| 11       | 202     | 2024-03-01 11:05:00 | scroll     | S002       | 400         |
| 12       | 202     | 2024-03-01 11:08:00 | click      | S002       | NULL        |
| 13       | 202     | 2024-03-01 11:10:00 | scroll     | S002       | 350         |
| 14       | 202     | 2024-03-01 11:15:00 | purchase   | S002       | 50          |
| 15       | 202     | 2024-03-01 11:20:00 | app_close  | S002       | NULL        |
| 16       | 203     | 2024-03-01 12:00:00 | app_open   | S003       | NULL        |
| 17       | 203     | 2024-03-01 12:10:00 | scroll     | S003       | 1000        |
| 18       | 203     | 2024-03-01 12:20:00 | scroll     | S003       | 1200        |
| 19       | 203     | 2024-03-01 12:25:00 | click      | S003       | NULL        |
| 20       | 203     | 2024-03-01 12:30:00 | scroll     | S003       | 800         |
| 21       | 203     | 2024-03-01 12:40:00 | scroll     | S003       | 900         |
| 22       | 203     | 2024-03-01 12:50:00 | scroll     | S003       | 1100        |
| 23       | 203     | 2024-03-01 13:00:00 | app_close  | S003       | NULL        |
| 24       | 204     | 2024-03-01 14:00:00 | app_open   | S004       | NULL        |
| 25       | 204     | 2024-03-01 14:05:00 | scroll     | S004       | 600         |
| 26       | 204     | 2024-03-01 14:08:00 | scroll     | S004       | 700         |
| 27       | 204     | 2024-03-01 14:10:00 | click      | S004       | NULL        |
| 28       | 204     | 2024-03-01 14:12:00 | app_close  | S004       | NULL        |
+----------+---------+---------------------+------------+------------+-------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------+---------+--------------------------+--------------+
| session_id | user_id | session_duration_minutes | scroll_count |
+------------+---------+--------------------------+--------------+
| S001       | 201     | 35                       | 6            |
+------------+---------+--------------------------+--------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Session S001 (User 201)</strong>:

    <ul>
       <li>Thời lượng: 10:00:00 đến 10:35:00 = 35 phút (lớn hơn 30)&nbsp;</li>
       <li>Số event scroll: 6 (ít nhất 5)&nbsp;</li>
       <li>Số event click: 0</li>
       <li>Tỷ lệ click trên scroll: 0/6 = 0.00 (nhỏ hơn 0.20)&nbsp;</li>
       <li>Số purchase: 0 (không có purchase)&nbsp;</li>
       <li>S001 là zombie session (thỏa mãn tất cả tiêu chí)</li>
    </ul>
    </li>
    <li><strong>Session S002 (User 202)</strong>:
    <ul>
       <li>Thời lượng: 11:00:00 đến 11:20:00 = 20 phút (nhỏ hơn 30)&nbsp;</li>
       <li>Có event purchase&nbsp;</li>
        <li>S002 không phải zombie session&nbsp;</li>
    </ul>
    </li>
    <li><strong>Session S003 (User 203)</strong>:
    <ul>
       <li>Thời lượng: 12:00:00 đến 13:00:00 = 60 phút (lớn hơn 30)&nbsp;</li>
       <li>Số event scroll: 5 (ít nhất 5)&nbsp;</li>
       <li>Số event click: 1</li>
       <li>Tỷ lệ click trên scroll: 1/5 = 0.20 (không nhỏ hơn 0.20)&nbsp;</li>
       <li>Số purchase: 0 (không có purchase)&nbsp;</li>
        <li>S003 không phải zombie session (tỷ lệ click trên scroll bằng 0.20, trong khi yêu cầu phải nhỏ hơn)</li>
    </ul>
    </li>
    <li><strong>Session S004 (User 204)</strong>:
    <ul>
       <li>Thời lượng: 14:00:00 đến 14:12:00 = 12 phút (nhỏ hơn 30)&nbsp;</li>
       <li>Số event scroll: 2 (nhỏ hơn 5)&nbsp;</li>
        <li>S004 không phải zombie session&nbsp;</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo scroll_count giảm dần, sau đó theo session_id tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng hợp theo nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Một zombie session được xác định bởi thời lượng, số scroll, tỷ lệ click trên scroll và việc không có purchase. Hãy nhóm theo $(\textit{session\_id},\textit{user\_id})$.
>
> Thời lượng là chênh lệch theo phút giữa timestamp cuối và đầu; ba loại event được đếm riêng.
>
> Giữ lại các session có thời lượng ít nhất $30$ phút, ít nhất năm scroll, tỷ lệ click nhỏ hơn $0.2$ và không có purchase, rồi sắp xếp theo số scroll giảm dần, sau đó theo session id.

<!-- thinking:end -->

Ta có thể nhóm các session theo session_id, tính thời lượng session, số event scroll, event click và event purchase của mỗi session, sau đó lọc theo các điều kiện đã cho trong đề bài. Cuối cùng, sắp xếp theo số event scroll giảm dần và theo session ID tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    session_id,
    user_id,
    TIMESTAMPDIFF(MINUTE, MIN(event_timestamp), MAX(event_timestamp)) session_duration_minutes,
    SUM(event_type = 'scroll') scroll_count
FROM app_events
GROUP BY session_id
HAVING
    session_duration_minutes >= 30
    AND SUM(event_type = 'click') / SUM(event_type = 'scroll') < 0.2
    AND SUM(event_type = 'purchase') = 0
    AND SUM(event_type = 'scroll') >= 5
ORDER BY scroll_count DESC, session_id;
```

#### Pandas

```python
import pandas as pd


def find_zombie_sessions(app_events: pd.DataFrame) -> pd.DataFrame:
    if not pd.api.types.is_datetime64_any_dtype(app_events["event_timestamp"]):
        app_events["event_timestamp"] = pd.to_datetime(app_events["event_timestamp"])

    grouped = app_events.groupby(["session_id", "user_id"])

    result = grouped.agg(
        session_duration_minutes=(
            "event_timestamp",
            lambda x: (x.max() - x.min()).total_seconds() // 60,
        ),
        scroll_count=("event_type", lambda x: (x == "scroll").sum()),
        click_count=("event_type", lambda x: (x == "click").sum()),
        purchase_count=("event_type", lambda x: (x == "purchase").sum()),
    ).reset_index()

    result = result[
        (result["session_duration_minutes"] >= 30)
        & (result["click_count"] / result["scroll_count"] < 0.2)
        & (result["purchase_count"] == 0)
        & (result["scroll_count"] >= 5)
    ]

    result = result.sort_values(
        by=["scroll_count", "session_id"], ascending=[False, True]
    ).reset_index(drop=True)

    return result[["session_id", "user_id", "session_duration_minutes", "scroll_count"]]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
