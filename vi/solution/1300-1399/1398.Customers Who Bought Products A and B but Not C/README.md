---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1398. Customers Who Bought Products A and B but Not C 🔒](https://leetcode.com/problems/customers-who-bought-products-a-and-b-but-not-c)

[中文文档](/solution/1300-1399/1398.Customers%20Who%20Bought%20Products%20A%20and%20B%20but%20Not%20C/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Customers</code></p>

<pre>
+---------------------+---------+
| Tên cột             | Kiểu    |
+---------------------+---------+
| customer_id         | int     |
| customer_name       | varchar |
+---------------------+---------+
customer_id là cột có các giá trị duy nhất trong bảng này.
customer_name là tên khách hàng.</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Orders</code></p>

<pre>
+---------------+---------+
| Tên cột       | Kiểu    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| product_name  | varchar |
+---------------+---------+
order_id là cột có các giá trị duy nhất trong bảng này.
customer_id là id của khách hàng đã mua sản phẩm &quot;product_name&quot;.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải&nbsp;để tìm customer_id và customer_name của những khách hàng đã mua sản phẩm <strong>&quot;A&quot;</strong>, <strong>&quot;B&quot;</strong> nhưng chưa mua sản phẩm <strong>&quot;C&quot;</strong>, vì chúng ta muốn đề xuất họ mua sản phẩm này.</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>customer_id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Customers:
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 1           | Daniel        |
| 2           | Diana         |
| 3           | Elizabeth     |
| 4           | Jhon          |
+-------------+---------------+
Bảng Orders:
+------------+--------------+---------------+
| order_id   | customer_id  | product_name  |
+------------+--------------+---------------+
| 10         |     1        |     A         |
| 20         |     1        |     B         |
| 30         |     1        |     D         |
| 40         |     1        |     C         |
| 50         |     2        |     A         |
| 60         |     3        |     A         |
| 70         |     3        |     B         |
| 80         |     3        |     D         |
| 90         |     4        |     C         |
+------------+--------------+---------------+
<strong>Đầu ra:</strong> 
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 3           | Elizabeth     |
+-------------+---------------+
<strong>Giải thích:</strong> Chỉ khách hàng có customer_id bằng 3 đã mua sản phẩm A và B nhưng chưa mua sản phẩm C.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: LEFT JOIN + GROUP BY + HAVING

<!-- thinking:start -->

> **Tư duy**
>
> Tìm khách hàng đã mua $A$ và $B$ nhưng chưa từng mua $C$. LEFT JOIN khách hàng với đơn hàng, group theo khách hàng, rồi yêu cầu $\mathrm{SUM}(\textit{product}=A)>0$, tương tự với $B$, và tổng của $C$ bằng 0.

<!-- thinking:end -->

Ta có thể dùng `LEFT JOIN` để nối bảng `Customers` với bảng `Orders`, sau đó group theo `customer_id`, rồi lọc ra những khách hàng đã mua sản phẩm A và B nhưng chưa mua sản phẩm C.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT customer_id, customer_name
FROM
    Customers
    LEFT JOIN Orders USING (customer_id)
GROUP BY 1
HAVING SUM(product_name = 'A') > 0 AND SUM(product_name = 'B') > 0 AND SUM(product_name = 'C') = 0
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
