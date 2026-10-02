---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1677. Product's Worth Over Invoices 🔒](https://leetcode.com/problems/products-worth-over-invoices)

[中文文档](/solution/1600-1699/1677.Product%27s%20Worth%20Over%20Invoices/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Product</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| name        | varchar |
+-------------+---------+
product_id là cột có giá trị duy nhất trong bảng này.
Bảng chứa ID và tên sản phẩm. Tên chỉ gồm các chữ cái tiếng Anh viết thường. Không có hai sản phẩm nào trùng tên.
</pre>

<p>&nbsp;</p>

<p>Table: <code>Invoice</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| invoice_id  | int  |
| product_id  | int  |
| rest        | int  |
| paid        | int  |
| canceled    | int  |
| refunded    | int  |
+-------------+------+
invoice_id là cột có giá trị duy nhất trong bảng này và là ID của hóa đơn.
product_id là ID của sản phẩm trong hóa đơn này.
rest là số tiền còn phải trả cho hóa đơn này.
paid là số tiền đã trả cho hóa đơn này.
canceled là số tiền đã hủy cho hóa đơn này.
refunded là số tiền đã hoàn lại cho hóa đơn này.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để với mọi sản phẩm, trả về tên sản phẩm cùng tổng số tiền còn phải trả, đã trả, đã hủy và đã hoàn lại trong tất cả hóa đơn.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>product_name</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong>
Product table:
+------------+-------+
| product_id | name  |
+------------+-------+
| 0          | ham   |
| 1          | bacon |
+------------+-------+
Invoice table:
+------------+------------+------+------+----------+----------+
| invoice_id | product_id | rest | paid | canceled | refunded |
+------------+------------+------+------+----------+----------+
| 23         | 0          | 2    | 0    | 5        | 0        |
| 12         | 0          | 0    | 4    | 0        | 3        |
| 1          | 1          | 1    | 1    | 0        | 1        |
| 2          | 1          | 1    | 0    | 1        | 1        |
| 3          | 1          | 0    | 1    | 1        | 1        |
| 4          | 1          | 1    | 1    | 1        | 0        |
+------------+------------+------+------+----------+----------+
<strong>Output:</strong>
+-------+------+------+----------+----------+
| name  | rest | paid | canceled | refunded |
+-------+------+------+----------+----------+
| bacon | 3    | 3    | 3        | 3        |
| ham   | 2    | 4    | 5        | 3        |
+-------+------+------+----------+----------+
<strong>Explanation:</strong>
- The amount of money left to pay for bacon is 1 + 1 + 0 + 1 = 3
- The amount of money paid for bacon is 1 + 0 + 1 + 1 = 3
- The amount of money canceled for bacon is 0 + 1 + 1 + 1 = 3
- The amount of money refunded for bacon is 1 + 1 + 1 + 0 = 3
- The amount of money left to pay for ham is 2 + 0 = 2
- The amount of money paid for ham is 0 + 4 = 4
- The amount of money canceled for ham is 5 + 0 = 5
- The amount of money refunded for ham is 0 + 3 = 3
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta tổng hợp rest/paid/canceled/refunded theo từng sản phẩm; các sản phẩm không có hóa đơn vẫn phải xuất hiện với giá trị 0.
>
> Left join $\texttt{Invoice}$ vào $\texttt{Product}$, dùng $\texttt{SUM}$ theo $\texttt{product\_id}$, thay các tổng null bằng $\texttt{IFNULL}(\ldots,0)$ và sắp xếp theo name.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    name,
    IFNULL(SUM(rest), 0) AS rest,
    IFNULL(SUM(paid), 0) AS paid,
    IFNULL(SUM(canceled), 0) AS canceled,
    IFNULL(SUM(refunded), 0) AS refunded
FROM
    Product
    LEFT JOIN Invoice USING (product_id)
GROUP BY product_id
ORDER BY name;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
