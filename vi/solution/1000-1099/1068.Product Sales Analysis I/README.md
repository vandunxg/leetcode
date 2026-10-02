---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1068. Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i)

[中文文档](/solution/1000-1099/1068.Product%20Sales%20Analysis%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Sales</code></p>

<pre>
+-------------+-------+
| Tên cột     | Kiểu  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
(sale_id, year) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
product_id là khóa ngoại (cột tham chiếu) đến bảng <code>Product</code>.
Mỗi hàng của bảng này ghi nhận một giao dịch bán sản phẩm product_id trong một năm nhất định.
Lưu ý giá là giá của mỗi đơn vị sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Product</code></p>

<pre>
+--------------+---------+
| Tên cột      | Kiểu    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
product_id là khóa chính của bảng này (cột có giá trị duy nhất).
Mỗi hàng của bảng này cho biết tên của một sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để lấy <code>product_name</code>, <code>year</code> và <code>price</code> cho mỗi <code>sale_id</code> trong bảng <code>Sales</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Sales:
+---------+------------+------+----------+-------+
| sale_id | product_id | year | quantity | price |
+---------+------------+------+----------+-------+ 
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |
+---------+------------+------+----------+-------+
Bảng Product:
+------------+--------------+
| product_id | product_name |
+------------+--------------+
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |
+------------+--------------+
<strong>Đầu ra:</strong> 
+--------------+-------+-------+
| product_name | year  | price |
+--------------+-------+-------+
| Nokia        | 2008  | 5000  |
| Nokia        | 2009  | 5000  |
| Apple        | 2011  | 9000  |
+--------------+-------+-------+
<strong>Giải thích:</strong> 
Từ sale_id = 1, ta biết Nokia được bán với giá 5000 vào năm 2008.
Từ sale_id = 2, ta biết Nokia được bán với giá 5000 vào năm 2009.
Từ sale_id = 7, ta biết Apple được bán với giá 9000 vào năm 2011.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Kết quả cần lấy năm và giá từ bảng sales cùng với tên sản phẩm, ghép theo `product_id`.
>
> Dùng inner join giữa `Sales` và `Product` để lấy `product_name, year, price`.
>
> `USING (product_id)` giúp không phải lặp lại cột dùng để join.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT product_name, year, price
FROM
    Sales
    JOIN Product USING (product_id);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
