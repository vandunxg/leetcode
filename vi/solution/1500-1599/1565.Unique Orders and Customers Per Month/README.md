---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1565. Unique Orders and Customers Per Month 🔒](https://leetcode.com/problems/unique-orders-and-customers-per-month)

[中文文档](/solution/1500-1599/1565.Unique%20Orders%20and%20Customers%20Per%20Month/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Orders</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| customer_id   | int     |
| invoice       | int     |
+---------------+---------+
order_id is the column with unique values for this table.
This table contains information about the orders made by customer_id.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm số <strong>order duy nhất</strong> và số <strong>customer duy nhất</strong> có invoice <strong>&gt; $20</strong> trong mỗi <strong>tháng khác nhau</strong>.</p>

<p>Trả về bảng kết quả được sắp xếp theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Orders table:
+----------+------------+-------------+------------+
| order_id | order_date | customer_id | invoice    |
+----------+------------+-------------+------------+
| 1        | 2020-09-15 | 1           | 30         |
| 2        | 2020-09-17 | 2           | 90         |
| 3        | 2020-10-06 | 3           | 20         |
| 4        | 2020-10-20 | 3           | 21         |
| 5        | 2020-11-10 | 1           | 10         |
| 6        | 2020-11-21 | 2           | 15         |
| 7        | 2020-12-01 | 4           | 55         |
| 8        | 2020-12-03 | 4           | 77         |
| 9        | 2021-01-07 | 3           | 31         |
| 10       | 2021-01-15 | 2           | 20         |
+----------+------------+-------------+------------+
<strong>Output:</strong> 
+---------+-------------+----------------+
| month   | order_count | customer_count |
+---------+-------------+----------------+
| 2020-09 | 2           | 2              |
| 2020-10 | 1           | 1              |
| 2020-12 | 2           | 1              |
| 2021-01 | 1           | 1              |
+---------+-------------+----------------+
<strong>Giải thích:</strong> 
Tháng 9 năm 2020 có hai order từ hai customer khác nhau với invoice &gt; $20.
Tháng 10 năm 2020 có hai order từ một customer, nhưng chỉ một order có invoice &gt; $20.
Tháng 11 năm 2020 có hai order từ hai customer khác nhau nhưng invoice &lt; $20, nên không đưa tháng này vào kết quả.
Tháng 12 năm 2020 có hai order từ một customer, cả hai đều có invoice &gt; $20.
Tháng 1 năm 2021 có hai order từ hai customer khác nhau, nhưng chỉ một order có invoice &gt; $20.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lọc có điều kiện + Thống kê theo nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi tháng, đếm các order có invoice lớn hơn $20$ và các customer khác nhau. Nhóm trực tiếp theo ngày sẽ tách một tháng thành nhiều ngày.
>
> Lọc $invoice>20$, định dạng ngày thành năm-tháng rồi tổng hợp: $COUNT$ order và $COUNT(DISTINCT\ customer\_id)$. Với Pandas, dùng $to\_period$ và $nunique$ để nhóm tương tự.

<!-- thinking:end -->

Trước tiên lọc các order có số tiền lớn hơn $20$, sau đó nhóm theo tháng để đếm số order và customer.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    DATE_FORMAT(order_date, '%Y-%m') AS month,
    COUNT(order_id) AS order_count,
    COUNT(DISTINCT customer_id) AS customer_count
FROM Orders
WHERE invoice > 20
GROUP BY 1;
```

#### Pandas

```python
import pandas as pd


def unique_orders_and_customers(orders: pd.DataFrame) -> pd.DataFrame:
    filtered_orders = orders[orders["invoice"] > 20]
    filtered_orders["month"] = (
        filtered_orders["order_date"].dt.to_period("M").astype(str)
    )
    result = (
        filtered_orders.groupby("month")
        .agg(
            order_count=("order_id", "count"), customer_count=("customer_id", "nunique")
        )
        .reset_index()
    )
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
