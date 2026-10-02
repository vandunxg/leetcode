---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1045. Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products)

[中文文档](/solution/1000-1099/1045.Customers%20Who%20Bought%20All%20Products/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Customer</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| customer_id | int     |
| product_key | int     |
+-------------+---------+
Bảng này có thể chứa các hàng trùng lặp. 
<code>customer_id</code> không được NULL<code>.</code>
product_key là khóa ngoại (cột tham chiếu) đến bảng <code>Product</code>.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Product</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_key | int     |
+-------------+---------+
product_key là khóa chính (cột có giá trị duy nhất) của bảng này.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm mã khách hàng trong bảng <code>Customer</code> đã mua tất cả sản phẩm trong bảng <code>Product</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Customer table:
+-------------+-------------+
| customer_id | product_key |
+-------------+-------------+
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |
+-------------+-------------+
Product table:
+-------------+
| product_key |
+-------------+
| 5           |
| 6           |
+-------------+
<strong>Output:</strong> 
+-------------+
| customer_id |
+-------------+
| 1           |
| 3           |
+-------------+
<strong>Giải thích:</strong> 
Các khách hàng đã mua tất cả sản phẩm (5 và 6) là khách hàng có mã 1 và 3.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và truy vấn con

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm khách hàng đã mua mọi sản phẩm trong danh mục. Sau khi nhóm theo khách hàng, số sản phẩm khác nhau họ đã mua phải bằng số hàng trong bảng sản phẩm.
>
> Nhóm bảng `Customer` theo `customer_id`, rồi so sánh `COUNT(DISTINCT product_key)` với một truy vấn con đếm số hàng trong `Product`.
>
> Truy vấn con tính tổng số sản phẩm chung một lần, thay vì ghép danh mục với từng hàng khách hàng.

<!-- thinking:end -->

Ta có thể nhóm bảng `Customer` theo `customer_id`, rồi dùng mệnh đề `HAVING` để lọc những khách hàng chưa mua đủ tất cả sản phẩm. Để làm vậy, ta dùng truy vấn con tìm tổng số sản phẩm khác nhau, sau đó so sánh con số này với số sản phẩm khác nhau mà mỗi khách hàng đã mua.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT customer_id
FROM Customer
GROUP BY 1
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(1) FROM Product);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
