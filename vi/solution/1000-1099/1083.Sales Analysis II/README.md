---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1083. Sales Analysis II 🔒](https://leetcode.com/problems/sales-analysis-ii)

[中文文档](/solution/1000-1099/1083.Sales%20Analysis%20II/README.md)

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
product_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi dòng trong bảng cho biết tên và giá của một sản phẩm.
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
Bảng này có thể chứa các dòng trùng lặp.
product_id là khóa ngoại (cột tham chiếu) đến bảng Product.
buyer_id không bao giờ là NULL. 
sale_date không bao giờ là NULL. 
Mỗi dòng trong bảng chứa thông tin về một giao dịch.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm các <strong>người mua</strong> đã mua <em>S8</em> nhưng chưa mua <em>iPhone</em>. Lưu ý, <em>S8</em> và <em>iPhone</em> là các sản phẩm có trong bảng <code>Product</code>.</p>

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
| 2         | 1          | 3        | 2019-06-02 | 1        | 800   |
| 3         | 3          | 3        | 2019-05-13 | 2        | 2800  |
+-----------+------------+----------+------------+----------+-------+
<strong>Đầu ra:</strong> 
+-------------+
| buyer_id    |
+-------------+
| 1           |
+-------------+
<strong>Giải thích:</strong> Người mua có id 1 đã mua S8 nhưng không mua iPhone. Người mua có id 3 đã mua cả hai sản phẩm.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm người mua đã mua S8 nhưng chưa từng mua iPhone. Mỗi người mua có thể có nhiều dòng dữ liệu, nên cần kiểm tra cả hai sản phẩm trong cùng một group.
>
> Join để lấy tên sản phẩm, group theo `buyer_id`, rồi yêu cầu `SUM(product_name = 'S8') > 0` đồng thời `SUM(product_name = 'iPhone') = 0`.
>
> Dùng conditional sum giúp tránh phải tính hai tập người mua riêng rồi lấy hiệu.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT buyer_id
FROM
    Sales
    JOIN Product USING (product_id)
GROUP BY 1
HAVING SUM(product_name = 'S8') > 0 AND SUM(product_name = 'iPhone') = 0;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
