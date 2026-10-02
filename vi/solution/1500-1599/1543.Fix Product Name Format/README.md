---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1543. Fix Product Name Format 🔒](https://leetcode.com/problems/fix-product-name-format)

[中文文档](/solution/1500-1599/1543.Fix%20Product%20Name%20Format/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Sales</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| sale_id      | int     |
| product_name | varchar |
| sale_date    | date    |
+--------------+---------+
sale_id is the column with unique values for this table.
Each row of this table contains the product name and the date it was sold.
</pre>

<p>&nbsp;</p>

<p>Vì bảng Sales được nhập thủ công vào năm <code>2000</code>, <code>product_name</code> có thể chứa khoảng trắng ở đầu hoặc cuối và không phân biệt hoa thường.</p>

<p>Hãy viết một lời giải để báo cáo</p>

<ul>
	<li><code>product_name</code> ở dạng chữ thường, không có khoảng trắng đầu hoặc cuối.</li>
	<li><code>sale_date</code> theo định dạng <code>(&#39;YYYY-MM&#39;)</code>.</li>
	<li><code>total</code> là số lần sản phẩm được bán trong tháng đó.</li>
</ul>

<p>Trả về bảng kết quả được sắp xếp theo <code>product_name</code> theo <strong>thứ tự tăng dần</strong>. Nếu bằng nhau, sắp xếp theo <code>sale_date</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Sales table:
+---------+--------------+------------+
| sale_id | product_name | sale_date  |
+---------+--------------+------------+
| 1       | LCPHONE      | 2000-01-16 |
| 2       | LCPhone      | 2000-01-17 |
| 3       | LcPhOnE      | 2000-02-18 |
| 4       | LCKeyCHAiN   | 2000-02-19 |
| 5       | LCKeyChain   | 2000-02-28 |
| 6       | Matryoshka   | 2000-03-31 |
+---------+--------------+------------+
<strong>Output:</strong> 
+--------------+-----------+-------+
| product_name | sale_date | total |
+--------------+-----------+-------+
| lckeychain   | 2000-02   | 2     |
| lcphone      | 2000-01   | 2     |
| lcphone      | 2000-02   | 1     |
| matryoshka   | 2000-03   | 1     |
+--------------+-----------+-------+
<strong>Explanation:</strong> 
In January, 2 LcPhones were sold. Please note that the product names are not case sensitive and may contain spaces.
In February, 2 LCKeychains and 1 LCPhone were sold.
In March, one matryoshka was sold.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tên sản phẩm có thể khác nhau về hoa thường và chứa khoảng trắng thừa, trong khi cần đếm doanh số theo tháng. Group trực tiếp cột gốc sẽ tách cùng một sản phẩm thành nhiều key.
>
> Xóa khoảng trắng và chuyển tên thành chữ thường, định dạng ngày thành năm-tháng, rồi group theo hai cột đó và đếm. CTE xử lý việc chuẩn hóa; query bên ngoài chỉ aggregate và sắp xếp.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            LOWER(TRIM(product_name)) AS product_name,
            DATE_FORMAT(sale_date, '%Y-%m') AS sale_date
        FROM Sales
    )
SELECT product_name, sale_date, COUNT(1) AS total
FROM t
GROUP BY 1, 2
ORDER BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
