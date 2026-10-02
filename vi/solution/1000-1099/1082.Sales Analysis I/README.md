---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1082. Sales Analysis I 🔒](https://leetcode.com/problems/sales-analysis-i)

[中文文档](/solution/1000-1099/1082.Sales%20Analysis%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Product</code></p>

<pre>
+--------------+---------+
| Tên cột      | Kiểu    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
| unit_price   | int     |
+--------------+---------+
product_id là khóa chính của bảng này (cột có giá trị duy nhất).
Mỗi hàng của bảng này cho biết tên và giá của một sản phẩm.
</pre>

<p>Bảng: <code>Sales</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| seller_id   | int     |
| product_id  | int     |
| buyer_id    | int     |
| sale_date   | date    |
| quantity    | int     |
| price       | int     |
+-------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
product_id là khóa ngoại (cột tham chiếu) đến bảng Product.
Mỗi hàng trong bảng này chứa thông tin về một giao dịch bán hàng.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm <strong>seller</strong> có tổng doanh thu cao nhất. Nếu có nhiều seller đồng hạng, hãy trả về tất cả.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Product:
+------------+--------------+------------+
| product_id | product_name | unit_price |
+------------+--------------+------------+
| 1          | S8           | 1000       |
| 2          | G4           | 800        |
| 3          | iPhone       | 1400       |
+------------+--------------+------------+
Bảng Sales:
+-----------+------------+----------+------------+----------+-------+
| seller_id | product_id | buyer_id | sale_date  | quantity | price |
+-----------+------------+----------+------------+----------+-------+
| 1         | 1          | 1        | 2019-01-21 | 2        | 2000  |
| 1         | 2          | 2        | 2019-02-17 | 1        | 800   |
| 2         | 2          | 3        | 2019-06-02 | 1        | 800   |
| 3         | 3          | 4        | 2019-05-13 | 2        | 2800  |
+-----------+------------+----------+------------+----------+-------+
<strong>Đầu ra:</strong> 
+-------------+
| seller_id   |
+-------------+
| 1           |
| 3           |
+-------------+
<strong>Giải thích:</strong> Seller có id 1 và 3 đều bán được sản phẩm với tổng giá bán cao nhất là 2800.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nhiều seller có thể cùng đạt doanh thu cao nhất. Sau khi tính tổng `price` theo từng `seller_id`, giữ lại những seller có tổng không thấp hơn bất kỳ nhóm nào khác.
>
> `HAVING SUM(price) >= ALL (grouped sums)` trả về tất cả seller đạt mức cao nhất.
>
> Tránh dùng `ORDER BY ... LIMIT 1` vì cách đó sẽ bỏ mất các trường hợp đồng hạng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT seller_id
FROM Sales
GROUP BY seller_id
HAVING
    SUM(price) >= ALL(
        SELECT SUM(price)
        FROM Sales
        GROUP BY seller_id
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Subquery dùng `ALL` phải thực hiện phép tổng hợp hai lần. Xếp hạng theo tổng giá giảm dần sẽ gán hạng $1$ cho tất cả seller đồng hạng cao nhất.
>
> CTE tính tổng và `rk`; truy vấn bên ngoài chỉ lấy các hàng có `rk = 1`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            seller_id,
            SUM(price) AS tot,
            RANK() OVER (ORDER BY SUM(price) DESC) AS rk
        FROM Sales
        GROUP BY seller_id
    )
SELECT seller_id
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
