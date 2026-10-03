---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2372. Calculate the Influence of Each Salesperson 🔒](https://leetcode.com/problems/calculate-the-influence-of-each-salesperson)

[中文文档](/solution/2300-2399/2372.Calculate%20the%20Influence%20of%20Each%20Salesperson/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Salesperson</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| salesperson_id | int     |
| name           | varchar |
+----------------+---------+
salesperson_id chứa các giá trị không trùng lặp.
Mỗi hàng trong bảng này cho biết ID của một nhân viên kinh doanh.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Customer</code></p>

<pre>
+----------------+------+
| Column Name    | Type |
+----------------+------+
| customer_id    | int  |
| salesperson_id | int  |
+----------------+------+
customer_id chứa các giá trị không trùng lặp.
salesperson_id là khóa ngoại (cột tham chiếu) của bảng Salesperson.
Mỗi hàng trong bảng này cho biết ID của một khách hàng và ID của nhân viên kinh doanh phụ trách khách hàng đó.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Sales</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| sale_id     | int  |
| customer_id | int  |
| price       | int  |
+-------------+------+
sale_id chứa các giá trị không trùng lặp.
customer_id là khóa ngoại (cột tham chiếu) của bảng Customer.
Mỗi hàng trong bảng này cho biết ID của một khách hàng và số tiền họ đã thanh toán cho giao dịch có sale_id.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo tổng số tiền mà khách hàng của mỗi nhân viên kinh doanh đã thanh toán. Nếu một nhân viên kinh doanh không có khách hàng nào, tổng giá trị phải là <code>0</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Salesperson:
+----------------+-------+
| salesperson_id | name  |
+----------------+-------+
| 1              | Alice |
| 2              | Bob   |
| 3              | Jerry |
+----------------+-------+
Bảng Customer:
+-------------+----------------+
| customer_id | salesperson_id |
+-------------+----------------+
| 1           | 1              |
| 2           | 1              |
| 3           | 2              |
+-------------+----------------+
Bảng Sales:
+---------+-------------+-------+
| sale_id | customer_id | price |
+---------+-------------+-------+
| 1       | 2           | 892   |
| 2       | 1           | 354   |
| 3       | 3           | 988   |
| 4       | 3           | 856   |
+---------+-------------+-------+
<strong>Đầu ra:</strong>
+----------------+-------+-------+
| salesperson_id | name  | total |
+----------------+-------+-------+
| 1              | Alice | 1246  |
| 2              | Bob   | 1844  |
| 3              | Jerry | 0     |
+----------------+-------+-------+
<strong>Giải thích:</strong>
Alice là nhân viên kinh doanh phụ trách khách hàng 1 và 2.
  - Khách hàng 1 đã thực hiện một giao dịch với giá 354.
  - Khách hàng 2 đã thực hiện một giao dịch với giá 892.
Tổng của Alice là 354 + 892 = 1246.

Bob là nhân viên kinh doanh phụ trách khách hàng 3.
  - Khách hàng 1 đã thực hiện hai giao dịch với giá 988 và 856.
Tổng của Bob là 988 + 856 = 1844.

Jerry không phụ trách khách hàng nào.
Tổng của Jerry là 0.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mức độ ảnh hưởng của một nhân viên kinh doanh là tổng số tiền từ các giao dịch của khách hàng của họ, hoặc $0$ nếu không có khách hàng nào. Những người không có giao dịch vẫn phải xuất hiện.
>
> LEFT JOIN $Customer$ rồi $Sales$ vào $Salesperson$, nhóm theo nhân viên kinh doanh và dùng $SUM(price)$ cùng $IFNULL$ để thay thế bằng $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT sp.salesperson_id, name, IFNULL(SUM(price), 0) AS total
FROM
    Salesperson AS sp
    LEFT JOIN Customer AS c ON sp.salesperson_id = c.salesperson_id
    LEFT JOIN Sales AS s ON s.customer_id = c.customer_id
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
