---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [262. Trips and Users](https://leetcode.com/problems/trips-and-users)

[中文文档](/solution/0200-0299/0262.Trips%20and%20Users/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Trips</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| client_id   | int      |
| driver_id   | int      |
| city_id     | int      |
| status      | enum     |
| request_at  | varchar  |     
+-------------+----------+
id là khóa chính (cột có giá trị duy nhất) của bảng này.
Bảng lưu thông tin tất cả chuyến taxi. Mỗi chuyến có một id duy nhất; client_id và driver_id là khóa ngoại tham chiếu đến users_id trong bảng Users.
Status có kiểu ENUM (danh mục) với các giá trị (&#39;completed&#39;, &#39;cancelled_by_driver&#39;, &#39;cancelled_by_client&#39;).
</pre>

<p> </p>

<p>Bảng: <code>Users</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| users_id    | int      |
| banned      | enum     |
| role        | enum     |
+-------------+----------+
users_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Bảng lưu thông tin tất cả người dùng. Mỗi người dùng có users_id duy nhất; role có kiểu ENUM với các giá trị (&#39;client&#39;, &#39;driver&#39;, &#39;partner&#39;).
banned có kiểu ENUM (danh mục) với các giá trị (&#39;Yes&#39;, &#39;No&#39;).
</pre>

<p> </p>

<p><strong>Tỷ lệ hủy</strong> được tính bằng số request bị hủy (bởi client hoặc driver) có người dùng không bị cấm, chia cho tổng số request trong ngày đó có người dùng không bị cấm.</p>

<p>Hãy viết lời giải để tìm <strong>tỷ lệ hủy</strong> mỗi ngày trong khoảng từ <code>&quot;2013-10-01&quot;</code> đến <code>&quot;2013-10-03&quot;</code>, chỉ tính các request có người dùng không bị cấm (<strong>cả client lẫn driver đều không bị cấm</strong>) và những ngày có <strong>ít nhất</strong> một chuyến đi. Làm tròn <code>Cancellation Rate</code> đến <strong>hai chữ số thập phân</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Trips:
+----+-----------+-----------+---------+---------------------+------------+
| id | client_id | driver_id | city_id | status              | request_at |
+----+-----------+-----------+---------+---------------------+------------+
| 1  | 1         | 10        | 1       | completed           | 2013-10-01 |
| 2  | 2         | 11        | 1       | cancelled_by_driver | 2013-10-01 |
| 3  | 3         | 12        | 6       | completed           | 2013-10-01 |
| 4  | 4         | 13        | 6       | cancelled_by_client | 2013-10-01 |
| 5  | 1         | 10        | 1       | completed           | 2013-10-02 |
| 6  | 2         | 11        | 6       | completed           | 2013-10-02 |
| 7  | 3         | 12        | 6       | completed           | 2013-10-02 |
| 8  | 2         | 12        | 12      | completed           | 2013-10-03 |
| 9  | 3         | 10        | 12      | completed           | 2013-10-03 |
| 10 | 4         | 13        | 12      | cancelled_by_driver | 2013-10-03 |
+----+-----------+-----------+---------+---------------------+------------+
Bảng Users:
+----------+--------+--------+
| users_id | banned | role   |
+----------+--------+--------+
| 1        | No     | client |
| 2        | Yes    | client |
| 3        | No     | client |
| 4        | No     | client |
| 10       | No     | driver |
| 11       | No     | driver |
| 12       | No     | driver |
| 13       | No     | driver |
+----------+--------+--------+
<strong>Đầu ra:</strong> 
+------------+-------------------+
| Day        | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 | 0.33              |
| 2013-10-02 | 0.00              |
| 2013-10-03 | 0.50              |
+------------+-------------------+
<strong>Giải thích:</strong> 
Ngày 2013-10-01:
  - Có tổng cộng 4 request, trong đó 2 request bị hủy.
  - Tuy nhiên, request có Id=2 do một client bị cấm (User_Id=2) gửi, nên không được tính.
  - Vì vậy, có tổng cộng 3 request hợp lệ, trong đó 1 request bị hủy.
  - Tỷ lệ hủy là (1 / 3) = 0.33
Ngày 2013-10-02:
  - Có tổng cộng 3 request, không có request nào bị hủy.
  - Request có Id=6 do một client bị cấm gửi, nên không được tính.
  - Vì vậy, có tổng cộng 2 request hợp lệ và không có request nào bị hủy.
  - Tỷ lệ hủy là (0 / 2) = 0.00
Ngày 2013-10-03:
  - Có tổng cộng 3 request, trong đó 1 request bị hủy.
  - Request có Id=8 do một client bị cấm gửi, nên không được tính.
  - Vì vậy, có tổng cộng 2 request hợp lệ, trong đó 1 request bị hủy.
  - Tỷ lệ hủy là (1 / 2) = 0.50
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính tỷ lệ hủy theo ngày trong ba ngày, chỉ tính các chuyến có client và driver không bị cấm. Join với $\textit{Users}$ hai lần để loại người dùng bị cấm.
>
> Group theo $request\_at$ và lấy $\mathrm{AVG}(\textit{status}\neq\texttt{completed})$, sau đó làm tròn đến hai chữ số thập phân.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def trips_and_users(trips: pd.DataFrame, users: pd.DataFrame) -> pd.DataFrame:
    # 1) temporal filtering
    trips = trips[trips["request_at"].between("2013-10-01", "2013-10-03")].rename(
        columns={"request_at": "Day"}
    )

    # 2) filtering based not banned
    # 2.1) mappning the column 'banned' to `client_id` and `driver_id`
    df_client = (
        pd.merge(trips, users, left_on="client_id", right_on="users_id", how="left")
        .drop(["users_id", "role"], axis=1)
        .rename(columns={"banned": "banned_client"})
    )
    df_driver = (
        pd.merge(trips, users, left_on="driver_id", right_on="users_id", how="left")
        .drop(["users_id", "role"], axis=1)
        .rename(columns={"banned": "banned_driver"})
    )
    df = pd.merge(
        df_client,
        df_driver,
        left_on=["id", "driver_id", "client_id", "city_id", "status", "Day"],
        right_on=["id", "driver_id", "client_id", "city_id", "status", "Day"],
        how="left",
    )
    # 2.2) filtering based on not banned
    df = df[(df["banned_client"] == "No") & (df["banned_driver"] == "No")]

    # 3) counting the cancelled and total trips per day
    df["status_cancelled"] = df["status"].str.contains("cancelled")
    df = df[["Day", "status_cancelled"]]
    df = df.groupby("Day").agg(
        {"status_cancelled": [("total_cancelled", "sum"), ("total", "count")]}
    )
    df.columns = df.columns.droplevel()
    df = df.reset_index()

    # 4) calculating the ratio
    df["Cancellation Rate"] = (df["total_cancelled"] / df["total"]).round(2)
    return df[["Day", "Cancellation Rate"]]
```

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    request_at AS Day,
    ROUND(AVG(status != 'completed'), 2) AS 'Cancellation Rate'
FROM
    Trips AS t
    JOIN Users AS u1 ON (t.client_id = u1.users_id AND u1.banned = 'No')
    JOIN Users AS u2 ON (t.driver_id = u2.users_id AND u2.banned = 'No')
WHERE request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY request_at;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
