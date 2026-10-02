---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1571. Warehouse Manager 🔒](https://leetcode.com/problems/warehouse-manager)

[中文文档](/solution/1500-1599/1571.Warehouse%20Manager/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Warehouse</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| name         | varchar |
| product_id   | int     |
| units        | int     |
+--------------+---------+
(name, product_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi dòng của bảng này chứa thông tin về các sản phẩm trong từng kho.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Products</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
| Width         | int     |
| Length        | int     |
| Height        | int     |
+---------------+---------+
product_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi dòng của bảng này chứa thông tin về kích thước sản phẩm (Width, Lenght và Height) tính bằng feet của từng sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo số feet khối mà <strong>thể tích</strong> hàng tồn kho chiếm trong mỗi kho.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả truy vấn như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Warehouse table:
+------------+--------------+-------------+
| name       | product_id   | units       |
+------------+--------------+-------------+
| LCHouse1   | 1            | 1           |
| LCHouse1   | 2            | 10          |
| LCHouse1   | 3            | 5           |
| LCHouse2   | 1            | 2           |
| LCHouse2   | 2            | 2           |
| LCHouse3   | 4            | 1           |
+------------+--------------+-------------+
Products table:
+------------+--------------+------------+----------+-----------+
| product_id | product_name | Width      | Length   | Height    |
+------------+--------------+------------+----------+-----------+
| 1          | LC-TV        | 5          | 50       | 40        |
| 2          | LC-KeyChain  | 5          | 5        | 5         |
| 3          | LC-Phone     | 2          | 10       | 10        |
| 4          | LC-T-Shirt   | 4          | 10       | 20        |
+------------+--------------+------------+----------+-----------+
<strong>Đầu ra:</strong> 
+----------------+------------+
| warehouse_name | volume     | 
+----------------+------------+
| LCHouse1       | 12250      | 
| LCHouse2       | 20250      |
| LCHouse3       | 800        |
+----------------+------------+
<strong>Giải thích:</strong> 
Thể tích của product_id = 1 (LC-TV), 5x50x40 = 10000
Thể tích của product_id = 2 (LC-KeyChain), 5x5x5 = 125 
Thể tích của product_id = 3 (LC-Phone), 2x10x10 = 200
Thể tích của product_id = 4 (LC-T-Shirt), 4x10x20 = 800
LCHouse1: 1 đơn vị LC-TV + 10 đơn vị LC-KeyChain + 5 đơn vị LC-Phone.
           Tổng thể tích: 1*10000 + 10*125  + 5*200 = 12250 feet khối
LCHouse2: 2 đơn vị LC-TV + 2 đơn vị LC-KeyChain.
           Tổng thể tích: 2*10000 + 2*125 = 20250 feet khối
LCHouse3: 1 đơn vị LC-T-Shirt.
           Tổng thể tích: 1*800 = 800 feet khối.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Inner Join + Group By + Sum Function

<!-- thinking:start -->

> **Tư duy**
>
> Tính tổng thể tích của mỗi kho. Thể tích là tích của các kích thước sản phẩm và số lượng lưu trữ; các cột này nằm trong hai bảng.
>
> Join theo $product\_id$, group theo tên kho, rồi tính tổng $width\times length\times height\times units$.

<!-- thinking:end -->

Ta có thể dùng inner join để nối bảng `Warehouse` và bảng `Products` theo điều kiện `product_id`, sau đó group theo tên kho để tính lượng hàng tồn kho của mỗi kho bằng hàm `SUM`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    name AS warehouse_name,
    SUM(width * length * height * units) AS volume
FROM
    Warehouse
    JOIN Products USING (product_id)
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
