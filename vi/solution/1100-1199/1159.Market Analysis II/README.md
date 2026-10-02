---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1159. Market Analysis II 🔒](https://leetcode.com/problems/market-analysis-ii)

[中文文档](/solution/1100-1199/1159.Market%20Analysis%20II/README.md)

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
user_id là primary key (cột có giá trị duy nhất) của bảng này.
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
order_id là primary key (cột có giá trị duy nhất) của bảng này.
item_id là foreign key (cột tham chiếu) đến bảng Items.
buyer_id và seller_id là foreign key tham chiếu đến bảng Users.
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
item_id là primary key (cột có giá trị duy nhất) của bảng này.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để xác định với mỗi người dùng, thương hiệu của sản phẩm thứ hai họ bán (theo ngày) có phải thương hiệu yêu thích của họ hay không. Nếu người dùng bán ít hơn hai sản phẩm, trả lời no. Đảm bảo không có seller nào bán nhiều hơn một sản phẩm trong cùng ngày.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Users:
+---------+------------+----------------+
| user_id | join_date  | favorite_brand |
+---------+------------+----------------+
| 1       | 2019-01-01 | Lenovo         |
| 2       | 2019-02-09 | Samsung        |
| 3       | 2019-01-19 | LG             |
| 4       | 2019-05-21 | HP             |
+---------+------------+----------------+
Bảng Orders:
+----------+------------+---------+----------+-----------+
| order_id | order_date | item_id | buyer_id | seller_id |
+----------+------------+---------+----------+-----------+
| 1        | 2019-08-01 | 4       | 1        | 2         |
| 2        | 2019-08-02 | 2       | 1        | 3         |
| 3        | 2019-08-03 | 3       | 2        | 3         |
| 4        | 2019-08-04 | 1       | 4        | 2         |
| 5        | 2019-08-04 | 1       | 3        | 4         |
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
<strong>Đầu ra:</strong> 
+-----------+--------------------+
| seller_id | 2nd_item_fav_brand |
+-----------+--------------------+
| 1         | no                 |
| 2         | yes                |
| 3         | yes                |
| 4         | no                 |
+-----------+--------------------+
<strong>Giải thích:</strong> 
Câu trả lời cho người dùng có id 1 là no vì họ không bán sản phẩm nào.
Câu trả lời cho người dùng có id 2 và 3 là yes vì thương hiệu của sản phẩm thứ hai họ bán trùng với thương hiệu yêu thích.
Câu trả lời cho người dùng có id 4 là no vì thương hiệu của sản phẩm thứ hai họ bán không phải thương hiệu yêu thích.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta so sánh thương hiệu của đơn hàng thứ hai mà mỗi seller bán với thương hiệu yêu thích của họ. Xếp hạng `orders` theo seller và `order_date`, giữ các hàng có $rk=2$, rồi left join users và items; nếu không có đơn hàng thứ hai thì kết quả là `no`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    u.user_id AS seller_id,
    CASE
        WHEN u.favorite_brand = i.item_brand THEN 'yes'
        ELSE 'no'
    END AS 2nd_item_fav_brand
FROM
    users AS u
    LEFT JOIN (
        SELECT
            order_date,
            item_id,
            seller_id,
            RANK() OVER (
                PARTITION BY seller_id
                ORDER BY order_date
            ) AS rk
        FROM orders
    ) AS o
        ON u.user_id = o.seller_id AND o.rk = 2
    LEFT JOIN items AS i ON o.item_id = i.item_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
