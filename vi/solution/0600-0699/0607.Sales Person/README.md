---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [607. Sales Person](https://leetcode.com/problems/sales-person)

[中文文档](/solution/0600-0699/0607.Sales%20Person/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>SalesPerson</code></p>

<pre>
+-----------------+---------+
| Column Name     | Type    |
+-----------------+---------+
| sales_id        | int     |
| name            | varchar |
| salary          | int     |
| commission_rate | int     |
| hire_date       | date    |
+-----------------+---------+
sales_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng ghi tên và ID của nhân viên bán hàng cùng với mức lương, tỷ lệ hoa hồng và ngày tuyển dụng.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Company</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| com_id      | int     |
| name        | varchar |
| city        | varchar |
+-------------+---------+
com_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng ghi tên, ID và thành phố nơi công ty tọa lạc.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Orders</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| order_date  | date |
| com_id      | int  |
| sales_id    | int  |
| amount      | int  |
+-------------+------+
order_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
com_id là khóa ngoại tham chiếu đến com_id trong bảng Company.
sales_id là khóa ngoại tham chiếu đến sales_id trong bảng SalesPerson.
Mỗi hàng trong bảng chứa thông tin về một đơn hàng, gồm ID công ty, ID nhân viên bán hàng, ngày đặt hàng và số tiền đã thanh toán.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm tên tất cả nhân viên bán hàng không có đơn hàng nào liên quan đến công ty có tên <strong>&quot;RED&quot;</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng SalesPerson:
+----------+------+--------+-----------------+------------+
| sales_id | name | salary | commission_rate | hire_date  |
+----------+------+--------+-----------------+------------+
| 1        | John | 100000 | 6               | 4/1/2006   |
| 2        | Amy  | 12000  | 5               | 5/1/2010   |
| 3        | Mark | 65000  | 12              | 12/25/2008 |
| 4        | Pam  | 25000  | 25              | 1/1/2005   |
| 5        | Alex | 5000   | 10              | 2/3/2007   |
+----------+------+--------+-----------------+------------+
Bảng Company:
+--------+--------+----------+
| com_id | name   | city     |
+--------+--------+----------+
| 1      | RED    | Boston   |
| 2      | ORANGE | New York |
| 3      | YELLOW | Boston   |
| 4      | GREEN  | Austin   |
+--------+--------+----------+
Bảng Orders:
+----------+------------+--------+----------+--------+
| order_id | order_date | com_id | sales_id | amount |
+----------+------------+--------+----------+--------+
| 1        | 1/1/2014   | 3      | 4        | 10000  |
| 2        | 2/1/2014   | 4      | 5        | 5000   |
| 3        | 3/1/2014   | 1      | 1        | 50000  |
| 4        | 4/1/2014   | 1      | 4        | 25000  |
+----------+------------+--------+----------+--------+
<strong>Đầu ra:</strong> 
+------+
| name |
+------+
| Amy  |
| Mark |
| Alex |
+------+
<strong>Giải thích:</strong> 
Dựa vào đơn hàng 3 và 4 trong bảng Orders, có thể thấy chỉ John và Pam có giao dịch bán hàng với công ty RED. Vì vậy, ta trả về tên của tất cả nhân viên bán hàng còn lại trong bảng SalesPerson.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: LEFT JOIN + GROUP BY

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm nhân viên chưa từng bán hàng cho công ty `RED`. Loại những người từng bán cho `RED` khỏi danh sách sẽ bỏ sót người chưa có đơn hàng nào.
>
> LEFT JOIN Orders và Company, nhóm theo nhân viên bán hàng, rồi giữ các hàng có `SUM` số đơn hàng của `RED` bằng $0$ (xem `NULL` như $0$).

<!-- thinking:end -->

Ta có thể LEFT JOIN bảng `SalesPerson` với bảng `Orders` theo sales id, rồi JOIN kết quả với bảng `Company` theo company id. Sau đó, nhóm theo `sales_id` và đếm số đơn hàng thuộc công ty có tên `RED`. Cuối cùng, lọc ra những nhân viên bán hàng không có đơn hàng nào với công ty `RED`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT s.name
FROM
    SalesPerson AS s
    LEFT JOIN Orders USING (sales_id)
    LEFT JOIN Company AS c USING (com_id)
GROUP BY sales_id
HAVING IFNULL(SUM(c.name = 'RED'), 0) = 0;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
