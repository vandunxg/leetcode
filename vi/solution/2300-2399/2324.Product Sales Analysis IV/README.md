---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2324. Product Sales Analysis IV 🔒](https://leetcode.com/problems/product-sales-analysis-iv)

[中文文档](/solution/2300-2399/2324.Product%20Sales%20Analysis%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Sales</code></p>

<pre>
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| user_id     | int   |
| quantity    | int   |
+-------------+-------+
sale_id chứa các giá trị duy nhất.
product_id là khóa ngoại (cột tham chiếu) đến bảng <code>Product</code>.
Mỗi hàng trong bảng này cho biết ID của sản phẩm và số lượng sản phẩm được một người dùng mua.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Product</code></p>

<pre>
+------------+------+
| Column Name | Type |
+------------+------+
| product_id | int  |
| price      | int  |
+------------+------+
product_id chứa các giá trị duy nhất.
Mỗi hàng trong bảng này cho biết giá của từng sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm product id mà mỗi người dùng đã chi nhiều tiền nhất. Nếu một người dùng đã chi nhiều tiền nhất cho từ hai sản phẩm trở lên, hãy trả về tất cả các sản phẩm đó.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Sales:
+---------+------------+---------+----------+
| sale_id | product_id | user_id | quantity |
+---------+------------+---------+----------+
| 1       | 1          | 101     | 10       |
| 2       | 3          | 101     | 7        |
| 3       | 1          | 102     | 9        |
| 4       | 2          | 102     | 6        |
| 5       | 3          | 102     | 10       |
| 6       | 1          | 102     | 6        |
+---------+------------+---------+----------+
Bảng Product:
+------------+-------+
| product_id | price |
+------------+-------+
| 1          | 10    |
| 2          | 25    |
| 3          | 15    |
+------------+-------+
<strong>Đầu ra:</strong>
+---------+------------+
| user_id | product_id |
+---------+------------+
| 101     | 3          |
| 102     | 1          |
| 102     | 2          |
| 102     | 3          |
+---------+------------+
<strong>Giải thích:</strong>
Người dùng 101:
    - Đã chi 10 * 10 = 100 cho sản phẩm 1.
    - Đã chi 7 * 15 = 105 cho sản phẩm 3.
Người dùng 101 đã chi nhiều tiền nhất cho sản phẩm 3.
Người dùng 102:
    - Đã chi (9 + 6) * 10 = 150 cho sản phẩm 1.
    - Đã chi 6 * 25 = 150 cho sản phẩm 2.
    - Đã chi 10 * 15 = 150 cho sản phẩm 3.
Người dùng 102 đã chi nhiều tiền nhất cho các sản phẩm 1, 2 và 3.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi người dùng, cần tìm sản phẩm có tổng tiền đã chi lớn nhất và vẫn giữ lại các trường hợp hòa. Hãy tính tổng $quantity \times price$ theo từng người dùng và sản phẩm, sau đó xếp hạng số tiền đã chi theo thứ tự giảm dần trong từng người dùng.
>
> Join $Sales$ với $Product$, nhóm dữ liệu, rồi giữ lại các hàng có hạng $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            user_id,
            product_id,
            RANK() OVER (
                PARTITION BY user_id
                ORDER BY SUM(quantity * price) DESC
            ) AS rk
        FROM
            Sales
            JOIN Product USING (product_id)
        GROUP BY 1, 2
    )
SELECT user_id, product_id
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
