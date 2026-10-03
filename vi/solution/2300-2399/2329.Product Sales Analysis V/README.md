---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2329. Product Sales Analysis V 🔒](https://leetcode.com/problems/product-sales-analysis-v)

[中文文档](/solution/2300-2399/2329.Product%20Sales%20Analysis%20V/README.md)

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
product_id là khóa ngoại (cột có các giá trị duy nhất) tham chiếu đến bảng <code>Product</code>.
Mỗi hàng trong bảng này cho biết ID của sản phẩm và số lượng sản phẩm được một người dùng mua.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Product</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| product_id  | int  |
| price       | int  |
+-------------+------+
product_id chứa các giá trị duy nhất.
Mỗi hàng trong bảng này cho biết giá của từng sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo số tiền chi tiêu của mỗi người dùng.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>spending</code> theo <strong>thứ tự giảm dần</strong>. Nếu có cùng số tiền, hãy sắp xếp theo <code>user_id</code> theo thứ tự tăng dần.</p>

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
| 2       | 2          | 101     | 1        |
| 3       | 3          | 102     | 3        |
| 4       | 3          | 102     | 2        |
| 5       | 2          | 103     | 3        |
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
+---------+----------+
| user_id | spending |
+---------+----------+
| 101     | 125      |
| 102     | 75       |
| 103     | 75       |
+---------+----------+
<strong>Giải thích:</strong>
Người dùng 101 đã chi 10 * 10 + 1 * 25 = 125.
Người dùng 102 đã chi 3 * 15 + 2 * 15 = 75.
Người dùng 103 đã chi 3 * 25 = 75.
Người dùng 102 và 103 chi cùng số tiền, nên ta phá vỡ thế hòa bằng ID của họ, trong khi người dùng 101 đứng đầu.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tổng số tiền mỗi người dùng đã chi, sắp xếp theo số tiền giảm dần và ID người dùng tăng dần. Hãy join bảng Sales với giá sản phẩm, rồi nhóm theo người dùng.
>
> Sau phép join, tính tổng $quantity \times price$, rồi dùng $ORDER\ BY$ để sắp xếp theo spending và $user\_id$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT user_id, SUM(quantity * price) AS spending
FROM
    Sales
    JOIN Product USING (product_id)
GROUP BY 1
ORDER BY 2 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
