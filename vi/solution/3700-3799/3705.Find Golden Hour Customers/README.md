---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3705. Find Golden Hour Customers](https://leetcode.com/problems/find-golden-hour-customers)

[Tài liệu tiếng Trung](/solution/3700-3799/3705.Find%20Golden%20Hour%20Customers/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>restaurant_orders</code></p>

<pre>
+------------------+----------+
| Column Name      | Type     |
+------------------+----------+
| order_id         | int      |
| customer_id      | int      |
| order_timestamp  | datetime |
| order_amount     | decimal  |
| payment_method   | varchar  |
| order_rating     | int      |
+------------------+----------+
order_id is the unique identifier for this table.
payment_method can be cash, card, or app.
order_rating is between 1 and 5, where 5 is the best (NULL if not rated).
order_timestamp contains both date and time information.
</pre>

<p>Hãy viết lời giải để tìm các <strong>khách hàng golden hour</strong> - những khách hàng thường xuyên đặt hàng trong giờ cao điểm và có mức độ hài lòng cao. Một khách hàng là <strong>khách hàng golden hour</strong> nếu thỏa mãn TẤT CẢ các tiêu chí sau:</p>

<ul>
	<li>Đã thực hiện <strong>ít nhất</strong> <code>3</code> đơn hàng.</li>
	<li><strong>Ít nhất</strong> <code>60%</code> số đơn hàng được đặt trong <strong>giờ cao điểm&nbsp;</strong>(<code>11:00</code>-<code>14:00</code> hoặc <code>18:00</code>-<code>21:00</code>).</li>
	<li><strong>Điểm đánh giá trung bình</strong> của các đơn hàng đã đánh giá ít nhất là <code>4.0,</code> làm tròn đến<code> 2 </code> chữ số thập phân.</li>
	<li>Đã đánh giá <strong>ít nhất</strong> <code>50%</code> số đơn hàng.</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>average_rating</code> <em>theo thứ tự <strong>giảm dần</strong>, sau đó theo</em> <code>customer_id</code>​​​​​​​ <em>theo thứ tự <strong>giảm dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng restaurant_orders:</p>

<pre class="example-io">
+----------+-------------+---------------------+--------------+----------------+--------------+
| order_id | customer_id | order_timestamp     | order_amount | payment_method | order_rating |
+----------+-------------+---------------------+--------------+----------------+--------------+
| 1        | 101         | 2024-03-01 12:30:00 | 25.50        | card           | 5            |
| 2        | 101         | 2024-03-02 19:15:00 | 32.00        | app            | 4            |
| 3        | 101         | 2024-03-03 13:45:00 | 28.75        | card           | 5            |
| 4        | 101         | 2024-03-04 20:30:00 | 41.00        | app            | NULL         |
| 5        | 102         | 2024-03-01 11:30:00 | 18.50        | cash           | 4            |
| 6        | 102         | 2024-03-02 12:00:00 | 22.00        | card           | 3            |
| 7        | 102         | 2024-03-03 15:30:00 | 19.75        | cash           | NULL         |
| 8        | 103         | 2024-03-01 19:00:00 | 55.00        | app            | 5            |
| 9        | 103         | 2024-03-02 20:45:00 | 48.50        | app            | 4            |
| 10       | 103         | 2024-03-03 18:30:00 | 62.00        | card           | 5            |
| 11       | 104         | 2024-03-01 10:00:00 | 15.00        | cash           | 3            |
| 12       | 104         | 2024-03-02 09:30:00 | 18.00        | cash           | 2            |
| 13       | 104         | 2024-03-03 16:00:00 | 20.00        | card           | 3            |
| 14       | 105         | 2024-03-01 12:15:00 | 30.00        | app            | 4            |
| 15       | 105         | 2024-03-02 13:00:00 | 35.50        | app            | 5            |
| 16       | 105         | 2024-03-03 11:45:00 | 28.00        | card           | 4            |
+----------+-------------+---------------------+--------------+----------------+--------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+--------------+----------------------+----------------+
| customer_id | total_orders | peak_hour_percentage | average_rating |
+-------------+--------------+----------------------+----------------+
| 103         | 3            | 100                  | 4.67           |
| 101         | 4            | 100                  | 4.67           |
| 105         | 3            | 100                  | 4.33           |
+-------------+--------------+----------------------+----------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Khách hàng 101</strong>:

    <ul>
       <li>Tổng số đơn hàng: 4 (ít nhất 3)&nbsp;</li>
       <li>Số đơn hàng trong giờ cao điểm: 4 trên 4 (12:30, 19:15, 13:45 và 20:30 đều trong giờ cao điểm)</li>
       <li>Tỷ lệ đơn hàng trong giờ cao điểm: 100% (ít nhất 60%)&nbsp;</li>
       <li>Số đơn hàng đã đánh giá: 3 trên 4 (75% số đơn đã được đánh giá)&nbsp;</li>
       <li>Điểm đánh giá trung bình: (5+4+5)/3 = 4.67 (ít nhất 4.0)&nbsp;</li>
       <li>Kết quả: <strong>Khách hàng golden hour</strong></li>
    </ul>
    </li>
    <li><strong>Khách hàng 102</strong>:
    <ul>
       <li>Tổng số đơn hàng: 3 (ít nhất 3)&nbsp;</li>
       <li>Số đơn hàng trong giờ cao điểm: 2 trên 3 (11:30, 12:00 trong giờ cao điểm; 15:30 thì không)</li>
       <li>Tỷ lệ đơn hàng trong giờ cao điểm: 2/3 = 66.67% (ít nhất 60%)&nbsp;</li>
       <li>Số đơn hàng đã đánh giá: 2 trên 3 (66.67% số đơn đã được đánh giá)&nbsp;</li>
       <li>Điểm đánh giá trung bình: (4+3)/2 = 3.5 (nhỏ hơn 4.0)&nbsp;</li>
       <li>Kết quả: <strong>Không phải khách hàng golden hour</strong> (điểm đánh giá trung bình quá thấp)</li>
    </ul>
    </li>
    <li><strong>Khách hàng 103</strong>:
    <ul>
       <li>Tổng số đơn hàng: 3 (ít nhất 3)&nbsp;</li>
       <li>Số đơn hàng trong giờ cao điểm: 3 trên 3 (19:00, 20:45, 18:30 đều trong giờ cao điểm buổi tối)</li>
       <li>Tỷ lệ đơn hàng trong giờ cao điểm: 3/3 = 100% (ít nhất 60%)&nbsp;</li>
       <li>Số đơn hàng đã đánh giá: 3 trên 3 (100% số đơn đã được đánh giá)&nbsp;</li>
       <li>Điểm đánh giá trung bình: (5+4+5)/3 = 4.67 (ít nhất 4.0)&nbsp;</li>
       <li>Kết quả: <strong>Khách hàng golden hour</strong></li>
    </ul>
    </li>
    <li><strong>Khách hàng 104</strong>:
    <ul>
       <li>Tổng số đơn hàng: 3 (ít nhất 3)&nbsp;</li>
       <li>Số đơn hàng trong giờ cao điểm: 0 trên 3 (10:00, 09:30, 16:00 đều ngoài giờ cao điểm)</li>
       <li>Tỷ lệ đơn hàng trong giờ cao điểm: 0/3 = 0% (nhỏ hơn 60%)&nbsp;</li>
       <li>Kết quả: <strong>Không phải khách hàng golden hour</strong> (không đủ số đơn hàng trong giờ cao điểm)</li>
    </ul>
    </li>
    <li><strong>Khách hàng 105</strong>:
    <ul>
       <li>Tổng số đơn hàng: 3 (ít nhất 3)&nbsp;</li>
       <li>Số đơn hàng trong giờ cao điểm: 3 trên 3 (12:15, 13:00, 11:45 đều trong giờ cao điểm buổi trưa)</li>
       <li>Tỷ lệ đơn hàng trong giờ cao điểm: 3/3 = 100% (ít nhất 60%)&nbsp;</li>
       <li>Số đơn hàng đã đánh giá: 3 trên 3 (100% số đơn đã được đánh giá)&nbsp;</li>
       <li>Điểm đánh giá trung bình: (4+5+4)/3 = 4.33 (ít nhất 4.0)&nbsp;</li>
       <li>Kết quả: <strong>Khách hàng golden hour</strong></li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo average_rating DESC, sau đó theo customer_id DESC.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tính thống kê

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện phụ thuộc vào số đơn hàng, tỷ lệ đơn trong giờ cao điểm, tỷ lệ đơn đã đánh giá và điểm đánh giá trung bình của từng khách hàng; việc tự viết vòng lặp dễ dẫn đến sai sót. Nhóm theo `customer_id` giúp tính các giá trị tổng hợp đó trong một lần duyệt, sau đó lọc theo các ngưỡng đã cho và sắp xếp theo điểm đánh giá trung bình cùng mã khách hàng.

<!-- thinking:end -->

Ta có thể nhóm các đơn hàng theo `customer_id` và tính tổng số đơn hàng, số đơn hàng trong giờ cao điểm, số đơn hàng đã đánh giá và điểm đánh giá trung bình cho từng khách hàng. Sau đó, ta lọc theo các điều kiện trong đề bài và sắp xếp theo điểm đánh giá trung bình giảm dần, tiếp theo là mã khách hàng giảm dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    customer_id,
    COUNT(1) total_orders,
    ROUND(
        SUM(
            TIME(order_timestamp) BETWEEN '11:00:00' AND '14:00:00'
            OR TIME(order_timestamp) BETWEEN '18:00:00' AND '21:00:00'
        ) / COUNT(1) * 100
    ) peak_hour_percentage,
    ROUND(AVG(order_rating), 2) average_rating
FROM restaurant_orders
GROUP BY customer_id
HAVING
    total_orders >= 3
    AND peak_hour_percentage >= 60
    AND average_rating >= 4.0
    AND SUM(order_rating IS NOT NULL) / total_orders >= 0.5
ORDER BY average_rating DESC, customer_id DESC;
```

#### Pandas

```python
import pandas as pd
import numpy as np


def find_golden_hour_customers(restaurant_orders: pd.DataFrame) -> pd.DataFrame:
    df = restaurant_orders.copy()
    df["order_timestamp"] = pd.to_datetime(df["order_timestamp"])
    df["is_peak_hour"] = df["order_timestamp"].dt.time.between(
        pd.to_datetime("11:00:00").time(), pd.to_datetime("14:00:00").time()
    ) | df["order_timestamp"].dt.time.between(
        pd.to_datetime("18:00:00").time(), pd.to_datetime("21:00:00").time()
    )
    grouped = (
        df.groupby("customer_id")
        .agg(
            total_orders=("order_timestamp", "count"),
            peak_hour_count=("is_peak_hour", "sum"),
            average_rating=("order_rating", lambda x: x.dropna().mean()),
            non_null_rating_count=("order_rating", lambda x: x.notna().sum()),
        )
        .reset_index()
    )
    grouped["average_rating"] = grouped["average_rating"].round(2)
    grouped["peak_hour_percentage"] = (
        grouped["peak_hour_count"] / grouped["total_orders"] * 100
    ).round()
    filtered = grouped[
        (grouped["total_orders"] >= 3)
        & (grouped["peak_hour_percentage"] >= 60)
        & (grouped["average_rating"] >= 4.0)
        & (grouped["non_null_rating_count"] / grouped["total_orders"] >= 0.5)
    ]
    filtered = filtered.sort_values(
        by=["average_rating", "customer_id"], ascending=[False, False]
    )
    return filtered[
        ["customer_id", "total_orders", "peak_hour_percentage", "average_rating"]
    ]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
