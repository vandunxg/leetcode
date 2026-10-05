---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3716. Find Churn Risk Customers](https://leetcode.com/problems/find-churn-risk-customers)

[中文文档](/solution/3700-3799/3716.Find%20Churn%20Risk%20Customers/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>subscription_events</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| event_id         | int     |
| user_id          | int     |
| event_date       | date    |
| event_type       | varchar |
| plan_name        | varchar |
| monthly_amount   | decimal |
+------------------+---------+
event_id là định danh duy nhất của bảng này.
event_type có thể là start, upgrade, downgrade hoặc cancel.
plan_name có thể là basic, standard, premium hoặc NULL (khi event_type là cancel).
monthly_amount biểu thị chi phí thuê bao hằng tháng sau sự kiện này.
Với các sự kiện cancel, monthly_amount bằng 0.
</pre>

<p>Hãy viết lời giải để <strong>tìm khách hàng có nguy cơ rời bỏ</strong> - những người dùng có dấu hiệu cảnh báo trước khi rời bỏ dịch vụ. Một người dùng được xem là <b>khách hàng có nguy cơ rời bỏ</b> nếu thỏa mãn TẤT CẢ các tiêu chí sau:</p>

<ul>
	<li>Hiện đang có <strong>thuê bao hoạt động</strong> (sự kiện cuối cùng của họ không phải là cancel).</li>
	<li>Đã thực hiện <strong>ít nhất một</strong> lần downgrade trong lịch sử thuê bao.</li>
	<li><strong>Doanh thu từ gói hiện tại</strong> nhỏ hơn <code>50%</code> doanh thu gói cao nhất trong lịch sử của họ.</li>
	<li>Đã đăng ký thuê bao trong <strong>ít nhất</strong> <code>60</code> ngày.</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>days_as_subscriber</code> <em>theo thứ tự <strong>giảm dần</strong>, sau đó theo</em> <code>user_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng subscription_events:</p>

<pre class="example-io">
+----------+---------+------------+------------+-----------+----------------+
| event_id | user_id | event_date | event_type | plan_name | monthly_amount |
+----------+---------+------------+------------+-----------+----------------+
| 1        | 501     | 2024-01-01 | start      | premium   | 29.99          |
| 2        | 501     | 2024-02-15 | downgrade  | standard  | 19.99          |
| 3        | 501     | 2024-03-20 | downgrade  | basic     | 9.99           |
| 4        | 502     | 2024-01-05 | start      | standard  | 19.99          |
| 5        | 502     | 2024-02-10 | upgrade    | premium   | 29.99          |
| 6        | 502     | 2024-03-15 | downgrade  | basic     | 9.99           |
| 7        | 503     | 2024-01-10 | start      | basic     | 9.99           |
| 8        | 503     | 2024-02-20 | upgrade    | standard  | 19.99          |
| 9        | 503     | 2024-03-25 | upgrade    | premium   | 29.99          |
| 10       | 504     | 2024-01-15 | start      | premium   | 29.99          |
| 11       | 504     | 2024-03-01 | downgrade  | standard  | 19.99          |
| 12       | 504     | 2024-03-30 | cancel     | NULL      | 0.00           |
| 13       | 505     | 2024-02-01 | start      | basic     | 9.99           |
| 14       | 505     | 2024-02-28 | upgrade    | standard  | 19.99          |
| 15       | 506     | 2024-01-20 | start      | premium   | 29.99          |
| 16       | 506     | 2024-03-10 | downgrade  | basic     | 9.99           |
+----------+---------+------------+------------+-----------+----------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+----------+--------------+------------------------+-----------------------+--------------------+
| user_id  | current_plan | current_monthly_amount | max_historical_amount | days_as_subscriber |
+----------+--------------+------------------------+-----------------------+--------------------+
| 501      | basic        | 9.99                   | 29.99                 | 79                 |
| 502      | basic        | 9.99                   | 29.99                 | 70                 |
+----------+--------------+------------------------+-----------------------+--------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Người dùng 501</strong>:

    <ul>
        <li>Đang hoạt động: Sự kiện cuối cùng là downgrade&nbsp;sang basic (chưa bị hủy)&nbsp;</li>
        <li>Có các lần downgrade: Có, 2 lần downgrade trong lịch sử&nbsp;</li>
        <li>Doanh thu hiện tại (9.99) so với mức cao nhất (29.99): 9.99/29.99 = 33.3% (nhỏ hơn 50%)&nbsp;</li>
        <li>Số ngày đăng ký thuê bao: Từ ngày 1 tháng 1 đến ngày 20 tháng 3 = 79 ngày (ít nhất 60)&nbsp;</li>
        <li>Kết quả: <strong>Khách hàng có nguy cơ rời bỏ</strong></li>
    </ul>
    </li>
    <li><strong>Người dùng 502</strong>:
    <ul>
        <li>Đang hoạt động: Sự kiện cuối cùng là downgrade&nbsp;sang basic (chưa bị hủy)&nbsp;</li>
        <li>Có các lần downgrade: Có, 1 lần downgrade trong lịch sử&nbsp;</li>
        <li>Doanh thu hiện tại (9.99) so với mức cao nhất (29.99): 9.99/29.99 = 33.3% (nhỏ hơn 50%)&nbsp;</li>
        <li>Số ngày đăng ký thuê bao: Từ ngày 5 tháng 1 đến ngày 15 tháng 3 = 70 ngày (ít nhất 60)&nbsp;</li>
        <li>Kết quả: <strong>Khách hàng có nguy cơ rời bỏ</strong></li>
    </ul>
    </li>
    <li><strong>Người dùng 503</strong>:
    <ul>
        <li>Đang hoạt động: Sự kiện cuối cùng là upgrade&nbsp;sang premium (chưa bị hủy)&nbsp;</li>
        <li>Có các lần downgrade: Không có downgrade nào trong lịch sử&nbsp;</li>
        <li>Kết quả: <strong>Không có nguy cơ</strong> (không có lịch sử downgrade)</li>
    </ul>
    </li>
    <li><strong>Người dùng 504</strong>:
    <ul>
        <li>Đang hoạt động: Sự kiện cuối cùng là cancel</li>
        <li>Kết quả: <strong>Không có nguy cơ</strong> (thuê bao đã bị hủy)</li>
    </ul>
    </li>
    <li><strong>Người dùng 505</strong>:
    <ul>
        <li>Đang hoạt động: Sự kiện cuối cùng là 'upgrade' sang standard (chưa bị hủy)&nbsp;</li>
        <li>Có các lần downgrade: Không có downgrade nào trong lịch sử&nbsp;</li>
        <li>Kết quả: <strong>Không có nguy cơ</strong> (không có lịch sử downgrade)</li>
    </ul>
    </li>
    <li><strong>Người dùng 506</strong>:
    <ul>
        <li>Đang hoạt động: Sự kiện cuối cùng là downgrade&nbsp;sang basic (chưa bị hủy)&nbsp;</li>
        <li>Có các lần downgrade: Có, 1 lần downgrade trong lịch sử&nbsp;</li>
        <li>Doanh thu hiện tại (9.99) so với mức cao nhất (29.99): 9.99/29.99 = 33.3% (nhỏ hơn 50%)&nbsp;</li>
        <li>Số ngày đăng ký thuê bao: Từ ngày 20 tháng 1 đến ngày 10 tháng 3 = 50 ngày (ít hơn 60)&nbsp;</li>
        <li>Kết quả: <strong>Không có nguy cơ</strong> (thời gian đăng ký thuê bao chưa đủ)</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo days_as_subscriber DESC, sau đó theo user_id ASC.</p>

<p><strong>Lưu ý:</strong> days_as_subscriber được tính từ ngày diễn ra sự kiện đầu tiên đến ngày diễn ra sự kiện cuối cùng của mỗi người dùng.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thống kê theo nhóm + Join + Lọc có điều kiện

<!-- thinking:start -->

> **Tư duy**
>
> Nguy cơ rời bỏ phụ thuộc vào cả sự kiện mới nhất và toàn bộ lịch sử thuê bao (ngày bắt đầu, mức phí cao nhất, số lần downgrade). Sắp xếp theo người dùng rồi lấy dòng cuối cùng của mỗi nhóm sẽ cho biết trạng thái hiện tại; sau đó join với các giá trị tổng hợp theo nhóm để lọc những người dùng chưa hủy thuê bao, đã downgrade, hiện trả ít hơn một nửa mức cao nhất và đã đăng ký thuê bao ít nhất $60$ ngày.

<!-- thinking:end -->

Trước tiên, chúng ta sử dụng hàm cửa sổ để lấy bản ghi cuối cùng của mỗi người dùng, sắp xếp theo ngày sự kiện và ID sự kiện theo thứ tự giảm dần, từ đó thu được thông tin về sự kiện mới nhất của mỗi người dùng. Sau đó, chúng ta nhóm và tổng hợp thông tin lịch sử thuê bao của từng người dùng, bao gồm ngày bắt đầu thuê bao, ngày diễn ra sự kiện cuối cùng, mức phí thuê bao cao nhất trong lịch sử và số sự kiện downgrade. Cuối cùng, chúng ta join thông tin sự kiện mới nhất với thống kê lịch sử và lọc theo các điều kiện được nêu trong đề bài để lấy danh sách khách hàng có nguy cơ rời bỏ.

<!-- tabs:start -->

#### MySQL

```sql
WITH
    user_with_last_event AS (
        SELECT
            s.*,
            ROW_NUMBER() OVER (
                PARTITION BY user_id
                ORDER BY event_date DESC, event_id DESC
            ) AS rn
        FROM subscription_events s
    ),
    user_history AS (
        SELECT
            user_id,
            MIN(event_date) AS start_date,
            MAX(event_date) AS last_event_date,
            MAX(monthly_amount) AS max_historical_amount,
            SUM(
                CASE
                    WHEN event_type = 'downgrade' THEN 1
                    ELSE 0
                END
            ) AS downgrade_count
        FROM subscription_events
        GROUP BY user_id
    ),
    latest_event AS (
        SELECT
            user_id,
            event_type AS last_event_type,
            plan_name AS current_plan,
            monthly_amount AS current_monthly_amount
        FROM user_with_last_event
        WHERE rn = 1
    )
SELECT
    l.user_id,
    l.current_plan,
    l.current_monthly_amount,
    h.max_historical_amount,
    DATEDIFF(h.last_event_date, h.start_date) AS days_as_subscriber
FROM
    latest_event l
    JOIN user_history h ON l.user_id = h.user_id
WHERE
    l.last_event_type <> 'cancel'
    AND h.downgrade_count >= 1
    AND l.current_monthly_amount < 0.5 * h.max_historical_amount
    AND DATEDIFF(h.last_event_date, h.start_date) >= 60
ORDER BY days_as_subscriber DESC, l.user_id ASC;
```

#### Pandas

```python
import pandas as pd


def find_churn_risk_customers(subscription_events: pd.DataFrame) -> pd.DataFrame:
    subscription_events["event_date"] = pd.to_datetime(
        subscription_events["event_date"]
    )
    subscription_events = subscription_events.sort_values(
        ["user_id", "event_date", "event_id"]
    )
    last_events = (
        subscription_events.groupby("user_id")
        .tail(1)[["user_id", "event_type", "plan_name", "monthly_amount"]]
        .rename(
            columns={
                "event_type": "last_event_type",
                "plan_name": "current_plan",
                "monthly_amount": "current_monthly_amount",
            }
        )
    )

    agg_df = (
        subscription_events.groupby("user_id")
        .agg(
            start_date=("event_date", "min"),
            last_event_date=("event_date", "max"),
            max_historical_amount=("monthly_amount", "max"),
            downgrade_count=("event_type", lambda x: (x == "downgrade").sum()),
        )
        .reset_index()
    )

    merged = pd.merge(agg_df, last_events, on="user_id", how="inner")
    merged["days_as_subscriber"] = (
        merged["last_event_date"] - merged["start_date"]
    ).dt.days

    result = merged[
        (merged["last_event_type"] != "cancel")
        & (merged["downgrade_count"] >= 1)
        & (merged["current_monthly_amount"] < 0.5 * merged["max_historical_amount"])
        & (merged["days_as_subscriber"] >= 60)
    ][
        [
            "user_id",
            "current_plan",
            "current_monthly_amount",
            "max_historical_amount",
            "days_as_subscriber",
        ]
    ]

    result = result.sort_values(
        ["days_as_subscriber", "user_id"], ascending=[False, True]
    ).reset_index(drop=True)
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
