---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1327. List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period)

[中文文档](/solution/1300-1399/1327.List%20the%20Products%20Ordered%20in%20a%20Period/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Products</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| product_id       | int     |
| product_name     | varchar |
| product_category | varchar |
+------------------+---------+
product_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Bảng này chứa thông tin về các sản phẩm của công ty.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Orders</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| order_date    | date    |
| unit          | int     |
+---------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
product_id là khóa ngoại (cột tham chiếu) đến bảng Products.
unit là số lượng sản phẩm được đặt vào ngày order_date.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm tên các sản phẩm có ít nhất <code>100</code> đơn vị được đặt trong <strong>tháng 2 năm 2020</strong> cùng số lượng tương ứng.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Products:
+-------------+-----------------------+------------------+
| product_id  | product_name          | product_category |
+-------------+-----------------------+------------------+
| 1           | Leetcode Solutions    | Book             |
| 2           | Jewels of Stringology | Book             |
| 3           | HP                    | Laptop           |
| 4           | Lenovo                | Laptop           |
| 5           | Leetcode Kit          | T-shirt          |
+-------------+-----------------------+------------------+
Bảng Orders:
+--------------+--------------+----------+
| product_id   | order_date   | unit     |
+--------------+--------------+----------+
| 1            | 2020-02-05   | 60       |
| 1            | 2020-02-10   | 70       |
| 2            | 2020-01-18   | 30       |
| 2            | 2020-02-11   | 80       |
| 3            | 2020-02-17   | 2        |
| 3            | 2020-02-24   | 3        |
| 4            | 2020-03-01   | 20       |
| 4            | 2020-03-04   | 30       |
| 4            | 2020-03-04   | 60       |
| 5            | 2020-02-25   | 50       |
| 5            | 2020-02-27   | 50       |
| 5            | 2020-03-01   | 50       |
+--------------+--------------+----------+
<strong>Đầu ra:</strong> 
+--------------------+---------+
| product_name       | unit    |
+--------------------+---------+
| Leetcode Solutions | 130     |
| Leetcode Kit       | 100     |
+--------------------+---------+
<strong>Giải thích:</strong> 
Sản phẩm có product_id = 1 được đặt tổng cộng (60 + 70) = 130 đơn vị trong tháng 2.
Sản phẩm có product_id = 2 được đặt tổng cộng 80 đơn vị trong tháng 2.
Sản phẩm có product_id = 3 được đặt tổng cộng (2 + 3) = 5 đơn vị trong tháng 2.
Sản phẩm có product_id = 4 không được đặt trong tháng 2 năm 2020.
Sản phẩm có product_id = 5 được đặt tổng cộng (50 + 50) = 100 đơn vị trong tháng 2.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tên sản phẩm có tổng số lượng đặt trong tháng 2 năm $2020$ ít nhất là $100$. Join bảng đơn hàng với bảng sản phẩm, lọc các đơn trong tháng đó bằng định dạng ngày, tính tổng $\textit{unit}$ theo từng sản phẩm, rồi dùng $\mathrm{HAVING}$ để giữ lại các hàng có tổng đạt ngưỡng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT product_name, SUM(unit) AS unit
FROM
    Orders AS o
    JOIN Products AS p ON o.product_id = p.product_id
WHERE DATE_FORMAT(order_date, '%Y-%m') = '2020-02'
GROUP BY o.product_id
HAVING unit >= 100;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
