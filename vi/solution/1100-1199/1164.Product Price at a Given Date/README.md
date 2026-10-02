---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1164. Product Price at a Given Date](https://leetcode.com/problems/product-price-at-a-given-date)

[中文文档](/solution/1100-1199/1164.Product%20Price%20at%20a%20Given%20Date/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Products</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+
(product_id, change_date) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng cho biết giá của một sản phẩm đã được đổi thành mức giá mới vào một ngày cụ thể.</pre>

<p>Ban đầu, tất cả sản phẩm đều có giá 10.</p>

<p>Hãy viết lời giải để tìm giá của tất cả sản phẩm vào ngày <code>2019-08-16</code>.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Bảng Products:
+------------+-----------+-------------+
| product_id | new_price | change_date |
+------------+-----------+-------------+
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |
+------------+-----------+-------------+
<strong>Output:</strong> 
+------------+-------+
| product_id | price |
+------------+-------+
| 2          | 50    |
| 1          | 35    |
| 3          | 10    |
+------------+-------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Subquery + Join

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi sản phẩm, lấy lần thay đổi giá gần nhất vào hoặc trước ngày $2019$-$08$-$16$; nếu không có thì giá là $10$. Subquery lấy `MAX(change_date)` trong khoảng thời gian đó cho từng sản phẩm rồi join lại để lấy giá. Danh sách sản phẩm duy nhất được LEFT JOIN với kết quả này để các sản phẩm chưa có giá khớp được gán giá $10$.

<!-- thinking:end -->

Ta có thể dùng subquery để tìm giá sau lần thay đổi gần nhất của từng sản phẩm vào hoặc trước ngày đã cho và lưu kết quả vào bảng `P`. Sau đó, lấy toàn bộ `product_id` vào bảng `T`. Cuối cùng, LEFT JOIN bảng `T` với bảng `P` theo `product_id` để thu được kết quả.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (SELECT DISTINCT product_id FROM Products),
    P AS (
        SELECT product_id, new_price AS price
        FROM Products
        WHERE
            (product_id, change_date) IN (
                SELECT product_id, MAX(change_date) AS change_date
                FROM Products
                WHERE change_date <= '2019-08-16'
                GROUP BY 1
            )
    )
SELECT product_id, IFNULL(price, 10) AS price
FROM
    T
    LEFT JOIN P USING (product_id);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 tìm lần thay đổi gần nhất bằng subquery tổng hợp. Cách 2 LEFT JOIN các lần thay đổi trong khoảng ngày cần xét, xếp hạng theo `change_date` giảm dần bằng `RANK` rồi giữ hàng có $rk=1$. Hàng không có lần thay đổi nào vẫn được xếp hạng $1$, và `IFNULL` điền giá $10$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT p1.product_id, new_price, change_date
        FROM
            (
                SELECT DISTINCT product_id
                FROM Products
            ) AS p1
            LEFT JOIN Products AS p2
                ON p1.product_id = p2.product_id AND p2.change_date <= '2019-08-16'
    ),
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY product_id
                ORDER BY change_date DESC
            ) AS rk
        FROM P
    )
SELECT product_id, IFNULL(new_price, 10) AS price
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
