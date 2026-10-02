---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1070. Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii)

[中文文档](/solution/1000-1099/1070.Product%20Sales%20Analysis%20III/README.md)

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
(sale_id, year) là primary key của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng ghi nhận một giao dịch bán sản phẩm trong một năm nhất định.
Một sản phẩm có thể có nhiều bản ghi bán hàng trong cùng một năm.
Lưu ý rằng giá là giá trên mỗi đơn vị sản phẩm.

</pre>

<p>Hãy tìm tất cả giao dịch bán hàng diễn ra trong <strong data-end="967" data-start="953">năm đầu tiên</strong> mỗi sản phẩm được bán.</p>

<ul data-end="1234" data-start="992">
	<li data-end="1078" data-start="992">
	<p data-end="1078" data-start="994">Với mỗi <code data-end="1015" data-start="1003">product_id</code>, xác định <code data-end="1045" data-start="1039">year</code> sớm nhất xuất hiện trong bảng <code data-end="1071" data-start="1064">Sales</code>.</p>
	</li>
	<li data-end="1140" data-start="1079">
	<p data-end="1140" data-start="1081">Trả về <strong data-end="1095" data-start="1088">tất cả</strong> bản ghi bán hàng của sản phẩm đó trong năm này.</p>
	</li>
</ul>

<p data-end="1234" data-start="1143">Trả về bảng gồm các cột: <strong>product_id</strong>,<strong> first_year</strong>, <strong>quantity, </strong>và<strong> price</strong>.<br />
Có thể trả kết quả theo bất kỳ thứ tự nào.</p>

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

<strong>Đầu ra:</strong> 
+------------+------------+----------+-------+
| product_id | first_year | quantity | price |
+------------+------------+----------+-------+ 
| 100        | 2008       | 10       | 5000  |
| 200        | 2011       | 15       | 9000  |
+------------+------------+----------+-------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi sản phẩm, ta cần lấy mọi hàng bán hàng trong năm đầu tiên. Tìm năm nhỏ nhất của từng sản phẩm, sau đó lấy quantity và price của năm đó.
>
> Subquery có GROUP BY trả về `(product_id, MIN(year))`; query bên ngoài lấy các hàng trong `Sales` có cặp giá trị nằm trong tập đó.
>
> Một sản phẩm có thể có nhiều hàng trong cùng năm, vì vậy không được gộp chúng thành một hàng tổng hợp duy nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    product_id,
    year AS first_year,
    quantity,
    price
FROM Sales
WHERE
    (product_id, year) IN (
        SELECT
            product_id,
            MIN(year) AS year
        FROM Sales
        GROUP BY product_id
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Correlated subquery tính toán riêng cho từng sản phẩm. Window function có thể xếp hạng các năm trong một lượt quét bảng.
>
> `RANK() OVER (PARTITION BY product_id ORDER BY year)` đánh dấu năm sớm nhất bằng hạng $1$; điều kiện lọc bên ngoài giữ lại `rk = 1`, bao gồm cả các trường hợp đồng hạng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY product_id
                ORDER BY year
            ) AS rk
        FROM Sales
    )
SELECT product_id, year AS first_year, quantity, price
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
