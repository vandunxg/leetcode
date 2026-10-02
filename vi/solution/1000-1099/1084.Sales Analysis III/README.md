---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1084. Sales Analysis III](https://leetcode.com/problems/sales-analysis-iii)

[中文文档](/solution/1000-1099/1084.Sales%20Analysis%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Product</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
| unit_price   | int     |
+--------------+---------+
product_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này cho biết tên và giá của một sản phẩm.
</pre>

<p>Bảng: <code>Sales</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
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

<p>Hãy viết lời giải để tìm các <strong>sản phẩm</strong> chỉ được bán trong quý đầu tiên của <code>2019</code>, tức là từ ngày <code>2019-01-01</code> đến hết ngày <code>2019-03-31</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Product table:
+------------+--------------+------------+
| product_id | product_name | unit_price |
+------------+--------------+------------+
| 1          | S8           | 1000       |
| 2          | G4           | 800        |
| 3          | iPhone       | 1400       |
+------------+--------------+------------+
Sales table:
+-----------+------------+----------+------------+----------+-------+
| seller_id | product_id | buyer_id | sale_date  | quantity | price |
+-----------+------------+----------+------------+----------+-------+
| 1         | 1          | 1        | 2019-01-21 | 2        | 2000  |
| 1         | 2          | 2        | 2019-02-17 | 1        | 800   |
| 2         | 2          | 3        | 2019-06-02 | 1        | 800   |
| 3         | 3          | 4        | 2019-05-13 | 2        | 2800  |
+-----------+------------+----------+------------+----------+-------+
<strong>Output:</strong> 
+-------------+--------------+
| product_id  | product_name |
+-------------+--------------+
| 1           | S8           |
+-------------+--------------+
<strong>Giải thích:</strong> 
Sản phẩm có id 1 chỉ được bán vào mùa xuân năm 2019.
Sản phẩm có id 2 được bán vào mùa xuân năm 2019, nhưng cũng được bán sau mùa xuân năm đó.
Sản phẩm có id 3 được bán sau mùa xuân năm 2019.
Ta chỉ trả về sản phẩm 1 vì đây là sản phẩm duy nhất chỉ được bán vào mùa xuân năm 2019.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sản phẩm phải được bán trong quý I năm 2019 và không được bán vào thời điểm nào khác. Tất cả ngày bán của sản phẩm phải nằm trong khoảng thời gian đó.
>
> Sau khi join để lấy tên sản phẩm, một nhóm hợp lệ khi và chỉ khi số hàng của nhóm bằng số hàng có ngày bán nằm trong khoảng $[2019\text{-}01\text{-}01,2019\text{-}03\text{-}31]$.
>
> `COUNT(1) = SUM(sale_date BETWEEN ...)` loại các sản phẩm không được bán và những sản phẩm có giao dịch ngoài khoảng thời gian này.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT product_id, product_name
FROM
    Sales
    JOIN Product USING (product_id)
GROUP BY 1
HAVING COUNT(1) = SUM(sale_date BETWEEN '2019-01-01' AND '2019-03-31');
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
