---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3056. Snaps Analysis 🔒](https://leetcode.com/problems/snaps-analysis)

[中文文档](/solution/3000-3099/3056.Snaps%20Analysis/README.md)

## Mô tả

<!-- description:start -->
<p>Bảng: <code>Activities</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| activity_id   | int     |
| user_id       | int     |
| activity_type | enum    |
| time_spent    | decimal |
+---------------+---------+
activity_id là cột chứa các giá trị duy nhất của bảng này.
activity_type là kiểu ENUM (phân loại) gồm (&#39;send&#39;, &#39;open&#39;).
Bảng này chứa mã hoạt động, mã người dùng, loại hoạt động và thời gian thực hiện.
</pre>

<p>Bảng: <code>Age</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| age_bucket  | enum |
+-------------+------+
user_id là cột chứa các giá trị duy nhất của bảng này.
age_bucket là kiểu ENUM (phân loại) gồm (&#39;21-25&#39;, &#39;26-30&#39;, &#39;31-35&#39;).
Bảng này chứa mã người dùng và nhóm tuổi.</pre>

<p>Hãy viết lời giải để tính <strong>tỷ lệ phần trăm</strong> tổng thời gian dành cho việc <strong>gửi</strong> và <strong>mở snap</strong> cho <strong>từng nhóm tuổi</strong>. Tỷ lệ phần trăm phải được <strong>làm tròn</strong> đến <code>2</code> chữ số thập phân.</p>

<p><em>Trả về bảng kết quả </em><em>theo <strong>bất kỳ</strong> thứ tự nào.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Activities:
+-------------+---------+---------------+------------+
| activity_id | user_id | activity_type | time_spent |
+-------------+---------+---------------+------------+
| 7274        | 123     | open          | 4.50       |
| 2425        | 123     | send          | 3.50       |
| 1413        | 456     | send          | 5.67       |
| 2536        | 456     | open          | 3.00       |
| 8564        | 456     | send          | 8.24       |
| 5235        | 789     | send          | 6.24       |
| 4251        | 123     | open          | 1.25       |
| 1435        | 789     | open          | 5.25       |
+-------------+---------+---------------+------------+
Bảng Age:
+---------+------------+
| user_id | age_bucket |
+---------+------------+
| 123     | 31-35      |
| 789     | 21-25      |
| 456     | 26-30      |
+---------+------------+
<strong>Đầu ra:</strong>
+------------+-----------+-----------+
| age_bucket | send_perc | open_perc |
+------------+-----------+-----------+
| 31-35      | 37.84     | 62.16     |
| 26-30      | 82.26     | 17.74     |
| 21-25      | 54.31     | 45.69     |
+------------+-----------+-----------+
<strong>Giải thích:</strong>
Với nhóm tuổi 31-35:
  - Chỉ có một người dùng thuộc nhóm này, với mã người dùng là 123.
  - Tổng thời gian người dùng này dành cho việc gửi snap là 3.50, còn thời gian mở snap là 4.50 + 1.25 = 5.75.
  - Tổng thời gian người dùng này dành cho các hoạt động là 3.50 + 5.75 = 9.25.
  - Do đó, tỷ lệ gửi snap là (3.50 / 9.25) * 100 = 37.84, còn tỷ lệ mở snap là (5.75 / 9.25) * 100 = 62.16.
Với nhóm tuổi 26-30:
  - Chỉ có một người dùng thuộc nhóm này, với mã người dùng là 456.
  - Tổng thời gian người dùng này dành cho việc gửi snap là 5.67 + 8.24 = 13.91, còn thời gian mở snap là 3.00.
  - Tổng thời gian người dùng này dành cho các hoạt động là 13.91 + 3.00 = 16.91.
  - Do đó, tỷ lệ gửi snap là (13.91 / 16.91) * 100 = 82.26, còn tỷ lệ mở snap là (3.00 / 16.91) * 100 = 17.74.
Với nhóm tuổi 21-25:
  - Chỉ có một người dùng thuộc nhóm này, với mã người dùng là 789.
  - Tổng thời gian người dùng này dành cho việc gửi snap là 6.24, còn thời gian mở snap là 5.25.
  - Tổng thời gian người dùng này dành cho các hoạt động là 6.24 + 5.25 = 11.49.
  - Do đó, tỷ lệ gửi snap là (6.24 / 11.49) * 100 = 54.31, còn tỷ lệ mở snap là (5.25 / 11.49) * 100 = 45.69.
Tất cả tỷ lệ phần trăm trong bảng kết quả được làm tròn đến hai chữ số thập phân.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + Tính tổng bằng Group By

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính tỷ lệ thời gian gửi/mở theo từng nhóm tuổi. Hoạt động và độ tuổi nằm ở hai bảng khác nhau, nên trước hết cần join các bảng theo người dùng.
>
> Tính tổng theo $(\textit{age\_bucket}, \textit{activity\_type})$ rồi pivot thành hai cột sẽ tránh phải dùng các phép tổng hợp có điều kiện riêng biệt.
>
> Sau khi merge, group và pivot, mỗi tỷ lệ phần trăm sử dụng tổng của hai cột làm mẫu số.

<!-- thinking:end -->

Ta có thể thực hiện equi-join để kết nối bảng `Activities` và bảng `Age` dựa trên `user_id`. Sau đó, group theo `age_bucket` và tính tỷ lệ phần trăm gửi và mở cho từng nhóm tuổi.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    age_bucket,
    ROUND(100 * SUM(IF(activity_type = 'send', time_spent, 0)) / SUM(time_spent), 2) AS send_perc,
    ROUND(100 * SUM(IF(activity_type = 'open', time_spent, 0)) / SUM(time_spent), 2) AS open_perc
FROM
    Activities
    JOIN Age USING (user_id)
GROUP BY 1;
```

#### Python3

```python
import pandas as pd


def snap_analysis(activities: pd.DataFrame, age: pd.DataFrame) -> pd.DataFrame:
    merged_df = pd.merge(activities, age, on="user_id")
    total_time_per_age_activity = (
        merged_df.groupby(["age_bucket", "activity_type"])["time_spent"]
        .sum()
        .reset_index()
    )
    pivot_df = total_time_per_age_activity.pivot(
        index="age_bucket", columns="activity_type", values="time_spent"
    ).reset_index()
    pivot_df = pivot_df.fillna(0)
    pivot_df["send_perc"] = round(
        100 * pivot_df["send"] / (pivot_df["send"] + pivot_df["open"]), 2
    )
    pivot_df["open_perc"] = round(
        100 * pivot_df["open"] / (pivot_df["send"] + pivot_df["open"]), 2
    )
    return pivot_df[["age_bucket", "send_perc", "open_perc"]]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
