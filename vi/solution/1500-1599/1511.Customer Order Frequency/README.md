---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1511. Customer Order Frequency 🔒](https://leetcode.com/problems/customer-order-frequency)

[中文文档](/solution/1500-1599/1511.Customer%20Order%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Customers</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| country       | varchar |
+---------------+---------+
customer_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa thông tin về khách hàng của công ty.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Product</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| description   | varchar |
| price         | int     |
+---------------+---------+
product_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa thông tin về các sản phẩm của công ty.
price là giá của sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Orders</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| product_id    | int     |
| order_date    | date    |
| quantity      | int     |
+---------------+---------+
order_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa thông tin về các đơn hàng của khách hàng.
customer_id là id của khách hàng đã mua &quot;quantity&quot; sản phẩm có id là &quot;product_id&quot;.
Order_date là ngày theo định dạng (&#39;YYYY-MM-DD&#39;) khi đơn hàng được giao.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo <code>customer_id</code> và <code>customer_name</code> của những khách hàng đã chi ít nhất <code>$100</code> trong mỗi tháng <strong>6 và 7 năm 2020</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được thể hiện trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Customers table:
+--------------+-----------+-------------+
| customer_id  | name      | country     |
+--------------+-----------+-------------+
| 1            | Winston   | USA         |
| 2            | Jonathan  | Peru        |
| 3            | Moustafa  | Egypt       |
+--------------+-----------+-------------+
Product table:
+--------------+-------------+-------------+
| product_id   | description | price       |
+--------------+-------------+-------------+
| 10           | LC Phone    | 300         |
| 20           | LC T-Shirt  | 10          |
| 30           | LC Book     | 45          |
| 40           | LC Keychain | 2           |
+--------------+-------------+-------------+
Orders table:
+--------------+-------------+-------------+-------------+-----------+
| order_id     | customer_id | product_id  | order_date  | quantity  |
+--------------+-------------+-------------+-------------+-----------+
| 1            | 1           | 10          | 2020-06-10  | 1         |
| 2            | 1           | 20          | 2020-07-01  | 1         |
| 3            | 1           | 30          | 2020-07-08  | 2         |
| 4            | 2           | 10          | 2020-06-15  | 2         |
| 5            | 2           | 40          | 2020-07-01  | 10        |
| 6            | 3           | 20          | 2020-06-24  | 2         |
| 7            | 3           | 30          | 2020-06-25  | 2         |
| 9            | 3           | 30          | 2020-05-08  | 3         |
+--------------+-------------+-------------+-------------+-----------+
<strong>Output:</strong> 
+--------------+------------+
| customer_id  | name       |  
+--------------+------------+
| 1            | Winston    |
+--------------+------------+
<strong>Giải thích:</strong> 
Winston đã chi $300 (300 * 1) in June and $100 ( 10 * 1 + 45 * 2) in July 2020.
Jonathan đã chi $600 (300 * 2) in June and $20 ( 2 * 10) in July 2020.
Moustafa đã chi $110 (10 * 2 + 45 * 2) in June and $0 in July 2020.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Join + Group By + Having

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần những khách hàng đã chi ít nhất $100$ trong cả tháng 6 và tháng 7 năm $2020$. Đơn hàng, giá và tên nằm trong ba bảng khác nhau, nên cần join các bảng trước khi tổng hợp chi tiêu theo từng người.
>
> Thực hiện equi-join giữa $Orders$, $Product$ và $Customers$, chỉ giữ năm $2020$. Group theo khách hàng và dùng tổng có điều kiện của $quantity\times price$ cho tháng 6 và tháng 7; $HAVING$ giữ lại những khách hàng đạt ngưỡng trong cả hai tháng.

<!-- thinking:end -->

Ta có thể dùng câu lệnh `JOIN` để nối bảng `Orders` với bảng `Product`, sau đó nối kết quả với bảng `Customers`. Ta lọc các bản ghi có `order_date` không thuộc năm $2020$, rồi dùng câu lệnh `GROUP BY` để nhóm dữ liệu theo `customer_id`. Cuối cùng, dùng câu lệnh `HAVING` để lọc những khách hàng có mức chi tiêu trong tháng 6 và tháng 7 lớn hơn hoặc bằng $100$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT customer_id, name
FROM
    Orders
    JOIN Product USING (product_id)
    JOIN Customers USING (customer_id)
WHERE YEAR(order_date) = 2020
GROUP BY 1
HAVING
    SUM(IF(MONTH(order_date) = 6, quantity * price, 0)) >= 100
    AND SUM(IF(MONTH(order_date) = 7, quantity * price, 0)) >= 100;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
