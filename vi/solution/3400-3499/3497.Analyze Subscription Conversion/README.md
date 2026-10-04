---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3497. Analyze Subscription Conversion](https://leetcode.com/problems/analyze-subscription-conversion)

[中文文档](/solution/3400-3499/3497.Analyze%20Subscription%20Conversion/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>UserActivity</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| user_id          | int     |
| activity_date    | date    |
| activity_type    | varchar |
| activity_duration| int     |
+------------------+---------+
(user_id, activity_date, activity_type) is the unique key for this table.
activity_type is one of (&#39;free_trial&#39;, &#39;paid&#39;, &#39;cancelled&#39;).
activity_duration is the number of minutes the user spent on the platform that day.
Each row represents a user&#39;s activity on a specific date.
</pre>

<p>Một dịch vụ subscription muốn phân tích các mẫu hành vi của người dùng. Công ty cung cấp <code>7</code> ngày <strong>dùng thử miễn phí</strong>, sau đó người dùng có thể đăng ký <strong>gói trả phí</strong> hoặc <strong>hủy</strong>. Hãy viết lời giải để:</p>

<ol>
	<li>Tìm những người dùng đã chuyển từ dùng thử miễn phí sang subscription trả phí</li>
	<li>Tính <strong>thời lượng hoạt động trung bình mỗi ngày</strong> của từng người dùng trong giai đoạn <strong>dùng thử miễn phí</strong> (làm tròn đến <code>2</code> chữ số thập phân)</li>
	<li>Tính <strong>thời lượng hoạt động trung bình mỗi ngày</strong> của từng người dùng trong giai đoạn <strong>subscription trả phí</strong> (làm tròn đến <code>2</code> chữ số thập phân)</li>
</ol>

<p><em>Trả về bảng kết quả được sắp xếp theo </em><code>user_id</code><em> theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng UserActivity:</p>

<pre class="example-io">
+---------+---------------+---------------+-------------------+
| user_id | activity_date | activity_type | activity_duration |
+---------+---------------+---------------+-------------------+
| 1       | 2023-01-01    | free_trial    | 45                |
| 1       | 2023-01-02    | free_trial    | 30                |
| 1       | 2023-01-05    | free_trial    | 60                |
| 1       | 2023-01-10    | paid          | 75                |
| 1       | 2023-01-12    | paid          | 90                |
| 1       | 2023-01-15    | paid          | 65                |
| 2       | 2023-02-01    | free_trial    | 55                |
| 2       | 2023-02-03    | free_trial    | 25                |
| 2       | 2023-02-07    | free_trial    | 50                |
| 2       | 2023-02-10    | cancelled     | 0                 |
| 3       | 2023-03-05    | free_trial    | 70                |
| 3       | 2023-03-06    | free_trial    | 60                |
| 3       | 2023-03-08    | free_trial    | 80                |
| 3       | 2023-03-12    | paid          | 50                |
| 3       | 2023-03-15    | paid          | 55                |
| 3       | 2023-03-20    | paid          | 85                |
| 4       | 2023-04-01    | free_trial    | 40                |
| 4       | 2023-04-03    | free_trial    | 35                |
| 4       | 2023-04-05    | paid          | 45                |
| 4       | 2023-04-07    | cancelled     | 0                 |
+---------+---------------+---------------+-------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+--------------------+-------------------+
| user_id | trial_avg_duration | paid_avg_duration |
+---------+--------------------+-------------------+
| 1       | 45.00              | 76.67             |
| 3       | 70.00              | 63.33             |
| 4       | 37.50              | 45.00             |
+---------+--------------------+-------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Người dùng 1:</strong>

    <ul>
    <li>Có 3 ngày dùng thử miễn phí với thời lượng lần lượt là 45, 30 và 60 phút.</li>
    <li>Thời lượng dùng thử trung bình: (45 + 30 + 60) / 3 = 45.00 phút.</li>
    <li>Có 3 ngày subscription trả phí với thời lượng lần lượt là 75, 90 và 65 phút.</li>
    <li>Thời lượng trả phí trung bình: (75 + 90 + 65) / 3 = 76.67 phút.</li>
    </ul>
    </li>
    <li><strong>Người dùng 2:</strong>
    <ul>
    <li>Có 3 ngày dùng thử miễn phí với thời lượng lần lượt là 55, 25 và 50 phút.</li>
    <li>Thời lượng dùng thử trung bình: (55 + 25 + 50) / 3 = 43.33 phút.</li>
    <li>Không chuyển sang subscription trả phí (chỉ có các hoạt động free_trial và cancelled).</li>
    <li>Không được đưa vào đầu ra vì người dùng này không chuyển sang gói trả phí.</li>
    </ul>
    </li>
    <li><strong>Người dùng 3:</strong>
    <ul>
    <li>Có 3 ngày dùng thử miễn phí với thời lượng lần lượt là 70, 60 và 80 phút.</li>
    <li>Thời lượng dùng thử trung bình: (70 + 60 + 80) / 3 = 70.00 phút.</li>
    <li>Có 3 ngày subscription trả phí với thời lượng lần lượt là 50, 55 và 85 phút.</li>
    <li>Thời lượng trả phí trung bình: (50 + 55 + 85) / 3 = 63.33 phút.</li>
    </ul>
    </li>
    <li><strong>Người dùng 4:</strong>
    <ul>
    <li>Có 2 ngày dùng thử miễn phí với thời lượng lần lượt là 40 và 35 phút.</li>
    <li>Thời lượng dùng thử trung bình: (40 + 35) / 2 = 37.50 phút.</li>
    <li>Có 1 ngày subscription trả phí với thời lượng 45 phút trước khi hủy.</li>
    <li>Thời lượng trả phí trung bình: 45.00 phút.</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả chỉ bao gồm những người dùng đã chuyển từ dùng thử miễn phí sang subscription trả phí (người dùng 1, 3 và 4), và được sắp xếp theo user_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm + Lọc có điều kiện + Equi-Join

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần tìm những người dùng có cả hoạt động dùng thử và trả phí, đồng thời tính thời lượng trung bình của mỗi loại. Các dòng cancelled được bỏ qua.
>
> Loại bỏ $\textit{cancelled}$, tính trung bình theo người dùng và loại hoạt động, rồi inner-join để chỉ giữ những người dùng xuất hiện ở cả hai phía.
>
> Cộng thêm một lượng rất nhỏ trước khi làm tròn đến hai chữ số thập phân để tránh các trường hợp biên của số thực; sau đó sắp xếp theo $\textit{user\_id}$.

<!-- thinking:end -->

Đầu tiên, chúng ta lọc dữ liệu trong bảng để loại bỏ tất cả các bản ghi có `activity_type` bằng `cancelled`. Sau đó, chúng ta nhóm dữ liệu còn lại theo `user_id` và `activity_type`, tính thời lượng `duration` cho mỗi nhóm, rồi lưu kết quả vào bảng `T`.

Tiếp theo, chúng ta lọc bảng `T` để lấy các bản ghi có `activity_type` là `free_trial` và `paid`, lần lượt lưu vào các bảng `F` và `P`. Cuối cùng, chúng ta thực hiện equi-join trên hai bảng này bằng `user_id`, lọc các trường cần thiết theo đề bài, rồi sắp xếp kết quả để tạo ra đầu ra cuối cùng.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT user_id, activity_type, ROUND(SUM(activity_duration) / COUNT(1), 2) duration
        FROM UserActivity
        WHERE activity_type != 'cancelled'
        GROUP BY user_id, activity_type
    ),
    F AS (
        SELECT user_id, duration trial_avg_duration
        FROM T
        WHERE activity_type = 'free_trial'
    ),
    P AS (
        SELECT user_id, duration paid_avg_duration
        FROM T
        WHERE activity_type = 'paid'
    )
SELECT user_id, trial_avg_duration, paid_avg_duration
FROM
    F
    JOIN P USING (user_id)
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def analyze_subscription_conversion(user_activity: pd.DataFrame) -> pd.DataFrame:
    df = user_activity[user_activity["activity_type"] != "cancelled"]

    df_grouped = (
        df.groupby(["user_id", "activity_type"])["activity_duration"]
        .mean()
        .add(0.0001)
        .round(2)
        .reset_index()
    )

    df_free_trial = (
        df_grouped[df_grouped["activity_type"] == "free_trial"]
        .rename(columns={"activity_duration": "trial_avg_duration"})
        .drop(columns=["activity_type"])
    )

    df_paid = (
        df_grouped[df_grouped["activity_type"] == "paid"]
        .rename(columns={"activity_duration": "paid_avg_duration"})
        .drop(columns=["activity_type"])
    )

    result = df_free_trial.merge(df_paid, on="user_id", how="inner").sort_values(
        "user_id"
    )

    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
