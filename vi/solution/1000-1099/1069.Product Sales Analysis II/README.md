---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1069. Product Sales Analysis II 🔒](https://leetcode.com/problems/product-sales-analysis-ii)

[中文文档](/solution/1000-1099/1069.Product%20Sales%20Analysis%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Sales</code></p>

<pre>
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
(sale_id, year) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
product_id là khóa ngoại (cột tham chiếu) đến bảng <code>Product</code>.
Mỗi hàng trong bảng này ghi nhận một giao dịch bán sản phẩm product_id trong một năm nhất định.
Lưu ý, price là giá của một đơn vị sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Product</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
product_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này cho biết tên của một sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tính tổng số lượng đã bán cho từng product id.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Sales table:
+---------+------------+------+----------+-------+
| sale_id | product_id | year | quantity | price |
+---------+------------+------+----------+-------+ 
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |
+---------+------------+------+----------+-------+
Product table:
+------------+--------------+
| product_id | product_name |
+------------+--------------+
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |
+------------+--------------+
<strong>Output:</strong> 
+--------------+----------------+
| product_id   | total_quantity |
+--------------+----------------+
| 100          | 22             |
| 200          | 15             |
+--------------+----------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tổng số lượng bán ra của mỗi sản phẩm là tổng `quantity` theo `product_id`.
>
> Nhóm theo `product_id` rồi tính tổng `quantity`.
>
> `GROUP BY 1` tham chiếu đến cột đầu tiên trong danh sách được chọn; không cần dùng bảng sản phẩm.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT product_id, SUM(quantity) AS total_quantity
FROM Sales
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
