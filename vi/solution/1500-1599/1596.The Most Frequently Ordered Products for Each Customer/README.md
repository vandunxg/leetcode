---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1596. The Most Frequently Ordered Products for Each Customer 🔒](https://leetcode.com/problems/the-most-frequently-ordered-products-for-each-customer)

[中文文档](/solution/1500-1599/1596.The%20Most%20Frequently%20Ordered%20Products%20for%20Each%20Customer/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Customers</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
+---------------+---------+
customer_id is the column with unique values for this table.
This table contains information about the customers.
</pre>

<p>&nbsp;</p>

<p>Table: <code>Orders</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| customer_id   | int     |
| product_id    | int     |
+---------------+---------+
order_id is the column with unique values for this table.
This table contains information about the orders made by customer_id.
No customer will order the same product <strong>more than once</strong> in a single day.</pre>

<p>&nbsp;</p>

<p>Table: <code>Products</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
| price         | int     |
+---------------+---------+
product_id is the column with unique values for this table.
This table contains information about the products.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm sản phẩm được đặt nhiều nhất cho mỗi khách hàng.</p>

<p>Bảng kết quả phải có <code>product_id</code> và <code>product_name</code> cho mỗi <code>customer_id</code> đã đặt ít nhất một đơn hàng.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> 
Customers table:
+-------------+-------+
| customer_id | name  |
+-------------+-------+
| 1           | Alice |
| 2           | Bob   |
| 3           | Tom   |
| 4           | Jerry |
| 5           | John  |
+-------------+-------+
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
| 7        | 2020-08-01 | 3           | 3          |
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
+-------------+------------+--------------+
| customer_id | product_id | product_name |
+-------------+------------+--------------+
| 1           | 2          | mouse        |
| 2           | 1          | keyboard     |
| 2           | 2          | mouse        |
| 2           | 3          | screen       |
| 3           | 3          | screen       |
| 4           | 1          | keyboard     |
+-------------+------------+--------------+
<strong>Giải thích:</strong> 
Alice (customer 1) ordered the mouse three times and the keyboard one time, so the mouse is the most frequently ordered product for them.
Bob (customer 2) ordered the keyboard, the mouse, and the screen one time, so those are the most frequently ordered products for them.
Tom (customer 3) only ordered the screen (two times), so that is the most frequently ordered product for them.
Jerry (customer 4) only ordered the keyboard (one time), so that is the most frequently ordered product for them.
John (customer 5) did not order anything, so we do not include them in the result table.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By + Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi khách hàng, liệt kê mọi sản phẩm đồng hạng có số lần đặt cao nhất. Ta cần gom nhóm theo khách hàng và sản phẩm, sau đó giữ tần suất cao nhất trong mỗi nhóm.
>
> Group by $customer\_id,product\_id$, dùng $RANK$ theo count giảm dần, giữ $rk=1$, rồi join $Products$ để lấy tên. $RANK$ giữ lại mọi sản phẩm đồng hạng.

<!-- thinking:end -->

Ta group bảng `Orders` theo `customer_id` và `product_id`, sau đó dùng window function `rank()`, gán thứ hạng cho mỗi `product_id` trong từng nhóm `customer_id` theo tần suất giảm dần. Cuối cùng, ta chọn `product_id` có hạng $1$ cho mỗi `customer_id`, tức sản phẩm được đặt nhiều nhất. Khóa phân nhóm là `customer_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            customer_id,
            product_id,
            RANK() OVER (
                PARTITION BY customer_id
                ORDER BY COUNT(1) DESC
            ) AS rk
        FROM Orders
        GROUP BY 1, 2
    )
SELECT customer_id, product_id, product_name
FROM
    T
    JOIN Products USING (product_id)
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
