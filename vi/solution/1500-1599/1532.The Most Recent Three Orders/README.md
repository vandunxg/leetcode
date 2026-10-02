---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1532. The Most Recent Three Orders 🔒](https://leetcode.com/problems/the-most-recent-three-orders)

[中文文档](/solution/1500-1599/1532.The%20Most%20Recent%20Three%20Orders/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Customers</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
+---------------+---------+
customer_id is the column with unique values for this table.
Bảng này chứa thông tin về khách hàng.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Orders</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| customer_id   | int     |
| cost          | int     |
+---------------+---------+
order_id is the column with unique values for this table.
Bảng này chứa thông tin về các đơn hàng do customer_id đặt.
Mỗi khách hàng có <strong>một đơn hàng mỗi ngày</strong>.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm ba đơn hàng gần đây nhất của mỗi người dùng. Nếu người dùng đặt ít hơn ba đơn hàng, trả về tất cả đơn hàng của họ.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>customer_name</code> theo <strong>thứ tự tăng dần</strong>, nếu hòa thì theo <code>customer_id</code> theo <strong>thứ tự tăng dần</strong>. Nếu vẫn hòa, sắp xếp theo <code>order_date</code> theo <strong>thứ tự giảm dần</strong>.</p>

<p>The&nbsp;result format is in the following example.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Customers table:
+-------------+-----------+
| customer_id | name      |
+-------------+-----------+
| 1           | Winston   |
| 2           | Jonathan  |
| 3           | Annabelle |
| 4           | Marwan    |
| 5           | Khaled    |
+-------------+-----------+
Orders table:
+----------+------------+-------------+------+
| order_id | order_date | customer_id | cost |
+----------+------------+-------------+------+
| 1        | 2020-07-31 | 1           | 30   |
| 2        | 2020-07-30 | 2           | 40   |
| 3        | 2020-07-31 | 3           | 70   |
| 4        | 2020-07-29 | 4           | 100  |
| 5        | 2020-06-10 | 1           | 1010 |
| 6        | 2020-08-01 | 2           | 102  |
| 7        | 2020-08-01 | 3           | 111  |
| 8        | 2020-08-03 | 1           | 99   |
| 9        | 2020-08-07 | 2           | 32   |
| 10       | 2020-07-15 | 1           | 2    |
+----------+------------+-------------+------+
<strong>Đầu ra:</strong> 
+---------------+-------------+----------+------------+
| customer_name | customer_id | order_id | order_date |
+---------------+-------------+----------+------------+
| Annabelle     | 3           | 7        | 2020-08-01 |
| Annabelle     | 3           | 3        | 2020-07-31 |
| Jonathan      | 2           | 9        | 2020-08-07 |
| Jonathan      | 2           | 6        | 2020-08-01 |
| Jonathan      | 2           | 2        | 2020-07-30 |
| Marwan        | 4           | 4        | 2020-07-29 |
| Winston       | 1           | 8        | 2020-08-03 |
| Winston       | 1           | 1        | 2020-07-31 |
| Winston       | 1           | 10       | 2020-07-15 |
+---------------+-------------+----------+------------+
<strong>Giải thích:</strong> 
Winston có 4 đơn hàng, ta loại đơn hàng ngày &quot;2020-06-10&quot; vì đây là đơn cũ nhất.
Annabelle chỉ có 2 đơn hàng nên ta trả về cả hai.
Jonathan có đúng 3 đơn hàng.
Marwan chỉ đặt hàng một lần.
Ta sắp xếp bảng kết quả theo customer_name tăng dần, theo customer_id tăng dần và theo order_date giảm dần nếu hòa.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể viết lời giải tổng quát cho <code>n</code> đơn hàng gần đây nhất không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê ba đơn hàng gần đây nhất của mỗi khách hàng cùng với tên. Chọn thủ công ba dòng sau khi sắp xếp sẽ không ổn định khi ngày bị trùng; window sẽ đánh số các dòng trong từng nhóm.
>
> Join $Orders$ to $Customers$ on $customer\_id$, then $ROW\_NUMBER$ partitioned by customer and ordered by $order\_date$ descending. Keep ranks at most $3$, and emit name, id, and date in the required order.

<!-- thinking:end -->

Ta có thể dùng equi-join để nối bảng `Customers` và `Orders` dựa trên `customer_id`, sau đó dùng window function `row_number()` để sắp xếp đơn hàng của mỗi khách hàng theo `order_date` giảm dần và gán số thứ tự cho từng đơn hàng. Cuối cùng, lọc các đơn hàng có số thứ tự nhỏ hơn hoặc bằng $3$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            ROW_NUMBER() OVER (
                PARTITION BY customer_id
                ORDER BY order_date DESC
            ) AS rk
        FROM
            Orders
            JOIN Customers USING (customer_id)
    )
SELECT name AS customer_name, customer_id, order_id, order_date
FROM T
WHERE rk <= 3
ORDER BY 1, 2, 4 DESC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
