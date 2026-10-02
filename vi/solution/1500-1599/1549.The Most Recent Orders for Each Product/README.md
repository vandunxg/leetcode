---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1549. The Most Recent Orders for Each Product 🔒](https://leetcode.com/problems/the-most-recent-orders-for-each-product)

[中文文档](/solution/1500-1599/1549.The%20Most%20Recent%20Orders%20for%20Each%20Product/README.md)

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
customer_id là cột có các giá trị duy nhất trong bảng này.
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
| product_id    | int     |
+---------------+---------+
order_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa thông tin về các order do customer_id thực hiện.
Trong một ngày, không có sản phẩm nào được cùng một user đặt <strong>nhiều hơn một lần</strong>.</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Products</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
| price         | int     |
+---------------+---------+
product_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa thông tin về các sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm (các) order gần đây nhất của mỗi sản phẩm.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>product_name</code> tăng dần; nếu hòa, sắp xếp theo <code>product_id</code> theo <strong>thứ tự tăng dần</strong>. Nếu vẫn hòa, sắp xếp theo <code>order_id</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
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
+----------+------------+-------------+------------+
| order_id | order_date | customer_id | product_id |
+----------+------------+-------------+------------+
| 1        | 2020-07-31 | 1           | 1          |
| 2        | 2020-07-30 | 2           | 2          |
| 3        | 2020-08-29 | 3           | 3          |
| 4        | 2020-07-29 | 4           | 1          |
| 5        | 2020-06-10 | 1           | 2          |
| 6        | 2020-08-01 | 2           | 1          |
| 7        | 2020-08-01 | 3           | 1          |
| 8        | 2020-08-03 | 1           | 2          |
| 9        | 2020-08-07 | 2           | 3          |
| 10       | 2020-07-15 | 1           | 2          |
+----------+------------+-------------+------------+
Products table:
+------------+--------------+-------+
| product_id | product_name | price |
+------------+--------------+-------+
| 1          | keyboard     | 120   |
| 2          | mouse        | 80    |
| 3          | screen       | 600   |
| 4          | hard disk    | 450   |
+------------+--------------+-------+
<strong>Output:</strong> 
+--------------+------------+----------+------------+
| product_name | product_id | order_id | order_date |
+--------------+------------+----------+------------+
| keyboard     | 1          | 6        | 2020-08-01 |
| keyboard     | 1          | 7        | 2020-08-01 |
| mouse        | 2          | 8        | 2020-08-03 |
| screen       | 3          | 3        | 2020-08-29 |
+--------------+------------+----------+------------+
<strong>Explanation:</strong> 
keyboard&#39;s order gần đây nhất là vào 2020-08-01 và được đặt hai lần trong ngày này.
mouse&#39;s order gần đây nhất là vào 2020-08-03 và chỉ được đặt một lần trong ngày này.
screen&#39;s order gần đây nhất là vào 2020-08-29 và chỉ được đặt một lần trong ngày này.
hard disk chưa từng được đặt nên không xuất hiện trong bảng kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi sản phẩm, cần lấy mọi order trong ngày gần nhất. Nhiều order có thể cùng ngày, nên $ROW\_NUMBER$ sẽ loại bỏ các trường hợp hòa.
>
> Join $Orders$ với $Products$, sau đó dùng $RANK$ partition theo sản phẩm và sắp xếp ngày giảm dần. Các dòng có $rk=1$ giữ lại mọi order trong ngày gần nhất; cuối cùng sắp xếp theo tên và các identifier.

<!-- thinking:end -->

Ta có thể dùng equi-join để nối bảng `Orders` và `Products` dựa trên `product_id`, sau đó dùng window function `rank()` để xếp hạng mỗi `product_id` trong bảng `Orders` theo `order_date` giảm dần. Cuối cùng, chọn các dòng có hạng $1$ cho mỗi `product_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY product_id
                ORDER BY order_date DESC
            ) AS rk
        FROM
            Orders
            JOIN Products USING (product_id)
    )
SELECT product_name, product_id, order_id, order_date
FROM T
WHERE rk = 1
ORDER BY 1, 2, 3;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
