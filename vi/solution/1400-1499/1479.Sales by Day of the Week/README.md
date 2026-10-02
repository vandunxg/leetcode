---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1479. Sales by Day of the Week 🔒](https://leetcode.com/problems/sales-by-day-of-the-week)

[中文文档](/solution/1400-1499/1479.Sales%20by%20Day%20of%20the%20Week/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Orders</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| order_date    | date    |
| item_id       | varchar |
| quantity      | int     |
+---------------+---------+
(ordered_id, item_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng này chứa thông tin về các đơn hàng đã được đặt.
order_date là ngày item_id được khách hàng có mã customer_id đặt mua.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Items</code></p>

<pre>
+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| item_id             | varchar |
| item_name           | varchar |
| item_category       | varchar |
+---------------------+---------+
item_id là khóa chính (cột có giá trị duy nhất) của bảng này.
item_name là tên của mặt hàng.
item_category là danh mục của mặt hàng.
</pre>

<p>&nbsp;</p>

<p>Bạn là chủ doanh nghiệp và muốn có báo cáo doanh số theo danh mục mặt hàng và ngày trong tuần.</p>

<p>Hãy viết lời giải để báo cáo số lượng mặt hàng đã được đặt trong mỗi danh mục vào từng <strong>ngày trong tuần</strong>.</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>category</code>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Orders:
+------------+--------------+-------------+--------------+-------------+
| order_id   | customer_id  | order_date  | item_id      | quantity    |
+------------+--------------+-------------+--------------+-------------+
| 1          | 1            | 2020-06-01  | 1            | 10          |
| 2          | 1            | 2020-06-08  | 2            | 10          |
| 3          | 2            | 2020-06-02  | 1            | 5           |
| 4          | 3            | 2020-06-03  | 3            | 5           |
| 5          | 4            | 2020-06-04  | 4            | 1           |
| 6          | 4            | 2020-06-05  | 5            | 5           |
| 7          | 5            | 2020-06-05  | 1            | 10          |
| 8          | 5            | 2020-06-14  | 4            | 5           |
| 9          | 5            | 2020-06-21  | 3            | 5           |
+------------+--------------+-------------+--------------+-------------+
Bảng Items:
+------------+----------------+---------------+
| item_id    | item_name      | item_category |
+------------+----------------+---------------+
| 1          | LC Alg. Book   | Book          |
| 2          | LC DB. Book    | Book          |
| 3          | LC SmarthPhone | Phone         |
| 4          | LC Phone 2020  | Phone         |
| 5          | LC SmartGlass  | Glasses       |
| 6          | LC T-Shirt XL  | T-Shirt       |
+------------+----------------+---------------+
<strong>Đầu ra:</strong>
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
| Category   | Monday    | Tuesday   | Wednesday | Thursday  | Friday    | Saturday  | Sunday    |
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
| Book       | 20        | 5         | 0         | 0         | 10        | 0         | 0         |
| Glasses    | 0         | 0         | 0         | 0         | 5         | 0         | 0         |
| Phone      | 0         | 0         | 5         | 1         | 0         | 0         | 10        |
| T-Shirt    | 0         | 0         | 0         | 0         | 0         | 0         | 0         |
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
<strong>Giải thích:</strong>
Vào thứ Hai (2020-06-01, 2020-06-08), tổng cộng 20 mặt hàng (10 + 10) thuộc danh mục Book (mã: 1, 2) đã được bán.
Vào thứ Ba (2020-06-02), tổng cộng 5 mặt hàng thuộc danh mục Book (mã: 1, 2) đã được bán.
Vào thứ Tư (2020-06-03), tổng cộng 5 mặt hàng thuộc danh mục Phone (mã: 3, 4) đã được bán.
Vào thứ Năm (2020-06-04), tổng cộng 1 mặt hàng thuộc danh mục Phone (mã: 3, 4) đã được bán.
Vào thứ Sáu (2020-06-05), 10 mặt hàng thuộc danh mục Book (mã: 1, 2) và 5 mặt hàng thuộc danh mục Glasses (mã: 5) đã được bán.
Không có mặt hàng nào được bán vào thứ Bảy.
Vào Chủ nhật (2020-06-14, 2020-06-21), tổng cộng 10 mặt hàng (5 + 5) thuộc danh mục Phone (mã: 3, 4) đã được bán.
Không có doanh số nào cho T-shirt.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi danh mục cần tổng số lượng từ thứ Hai đến Chủ nhật, bao gồm cả những danh mục không có đơn hàng. Sử dụng `RIGHT JOIN` giữa `Orders` và `Items`, phân loại `quantity` bằng `DAYOFWEEK`, rồi nhóm theo danh mục.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    item_category AS category,
    SUM(IF(DAYOFWEEK(order_date) = '2', quantity, 0)) AS Monday,
    SUM(IF(DAYOFWEEK(order_date) = '3', quantity, 0)) AS Tuesday,
    SUM(IF(DAYOFWEEK(order_date) = '4', quantity, 0)) AS Wednesday,
    SUM(IF(DAYOFWEEK(order_date) = '5', quantity, 0)) AS Thursday,
    SUM(IF(DAYOFWEEK(order_date) = '6', quantity, 0)) AS Friday,
    SUM(IF(DAYOFWEEK(order_date) = '7', quantity, 0)) AS Saturday,
    SUM(IF(DAYOFWEEK(order_date) = '1', quantity, 0)) AS Sunday
FROM
    Orders AS o
    RIGHT JOIN Items AS i ON o.item_id = i.item_id
GROUP BY category
ORDER BY category;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
