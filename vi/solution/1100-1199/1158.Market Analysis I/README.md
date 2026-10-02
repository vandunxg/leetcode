---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1158. Market Analysis I](https://leetcode.com/problems/market-analysis-i)

[中文文档](/solution/1100-1199/1158.Market%20Analysis%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Users</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| join_date      | date    |
| favorite_brand | varchar |
+----------------+---------+
user_id là khóa chính của bảng này (cột có giá trị duy nhất).
Bảng này lưu thông tin người dùng của một website mua sắm trực tuyến, nơi người dùng có thể bán và mua sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Orders</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| item_id       | int     |
| buyer_id      | int     |
| seller_id     | int     |
+---------------+---------+
order_id là khóa chính của bảng này (cột có giá trị duy nhất).
item_id là khóa ngoại (cột tham chiếu) đến bảng Items.
buyer_id và seller_id là các khóa ngoại tham chiếu đến bảng Users.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Items</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| item_id       | int     |
| item_brand    | varchar |
+---------------+---------+
item_id là khóa chính của bảng này (cột có giá trị duy nhất).
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm ngày tham gia và số đơn hàng mỗi người dùng đặt với vai trò người mua trong năm <code>2019</code>.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Bảng Users:
+---------+------------+----------------+
| user_id | join_date  | favorite_brand |
+---------+------------+----------------+
| 1       | 2018-01-01 | Lenovo         |
| 2       | 2018-02-09 | Samsung        |
| 3       | 2018-01-19 | LG             |
| 4       | 2018-05-21 | HP             |
+---------+------------+----------------+
Bảng Orders:
+----------+------------+---------+----------+-----------+
| order_id | order_date | item_id | buyer_id | seller_id |
+----------+------------+---------+----------+-----------+
| 1        | 2019-08-01 | 4       | 1        | 2         |
| 2        | 2018-08-02 | 2       | 1        | 3         |
| 3        | 2019-08-03 | 3       | 2        | 3         |
| 4        | 2018-08-04 | 1       | 4        | 2         |
| 5        | 2018-08-04 | 1       | 3        | 4         |
| 6        | 2019-08-05 | 2       | 2        | 4         |
+----------+------------+---------+----------+-----------+
Bảng Items:
+---------+------------+
| item_id | item_brand |
+---------+------------+
| 1       | Samsung    |
| 2       | Lenovo     |
| 3       | LG         |
| 4       | HP         |
+---------+------------+
<strong>Output:</strong> 
+-----------+------------+----------------+
| buyer_id  | join_date  | orders_in_2019 |
+-----------+------------+----------------+
| 1         | 2018-01-01 | 1              |
| 2         | 2018-02-09 | 2              |
| 3         | 2018-01-19 | 0              |
| 4         | 2018-05-21 | 0              |
+-----------+------------+----------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mọi người dùng đều phải xuất hiện trong kết quả, kể cả khi không có đơn hàng nào trong năm $2019$. LEFT JOIN với `Orders`, giới hạn theo người mua và năm $2019$, rồi `COUNT` theo từng người dùng; người dùng không khớp đơn hàng sẽ có giá trị $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    u.user_id AS buyer_id,
    u.join_date,
    COUNT(order_id) AS orders_in_2019
FROM
    Users AS u
    LEFT JOIN Orders AS o ON u.user_id = o.buyer_id AND YEAR(order_date) = 2019
GROUP BY user_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 đặt điều kiện năm trong phép JOIN. Cách 2 JOIN tất cả đơn hàng rồi tính `SUM(YEAR(order_date)=2019)`; các năm khác đóng góp $0$, còn `IFNULL` xử lý người dùng không có đơn hàng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    user_id AS buyer_id,
    join_date,
    IFNULL(SUM(YEAR(order_date) = 2019), 0) AS orders_in_2019
FROM
    Users AS u
    LEFT JOIN Orders AS o ON u.user_id = buyer_id
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
