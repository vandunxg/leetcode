---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1364. Number of Trusted Contacts of a Customer 🔒](https://leetcode.com/problems/number-of-trusted-contacts-of-a-customer)

[中文文档](/solution/1300-1399/1364.Number%20of%20Trusted%20Contacts%20of%20a%20Customer/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Customers</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| customer_name | varchar |
| email         | varchar |
+---------------+---------+
customer_id là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng trong bảng chứa tên và email của một khách hàng tại cửa hàng trực tuyến.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Contacts</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | id      |
| contact_name  | varchar |
| contact_email | varchar |
+---------------+---------+
(user_id, contact_email) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng trong bảng chứa tên và email của một liên hệ thuộc khách hàng có user_id tương ứng.
Bảng này chứa thông tin về những người mà mỗi khách hàng tin tưởng. Người liên hệ có thể có hoặc không có trong bảng Customers.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Invoices</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| invoice_id   | int     |
| price        | int     |
| user_id      | int     |
+--------------+---------+
invoice_id là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng cho biết user_id có một hóa đơn với invoice_id và mức giá tương ứng.
</pre>

<p>&nbsp;</p>

<p>Viết truy vấn để tìm các thông tin sau cho mỗi <code>invoice_id</code>:</p>

<ul>
	<li><code>customer_name</code>: Tên khách hàng liên quan đến hóa đơn.</li>
	<li><code>price</code>: Giá của hóa đơn.</li>
	<li><code>contacts_cnt</code>: Số liên hệ của khách hàng.</li>
	<li><code>trusted_contacts_cnt</code>: Số liên hệ của khách hàng đồng thời cũng là khách hàng của cửa hàng (tức email của họ có trong bảng <code>Customers</code>).</li>
</ul>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>invoice_id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Customers table:
+-------------+---------------+--------------------+
| customer_id | customer_name | email              |
+-------------+---------------+--------------------+
| 1           | Alice         | alice@leetcode.com |
| 2           | Bob           | bob@leetcode.com   |
| 13          | John          | john@leetcode.com  |
| 6           | Alex          | alex@leetcode.com  |
+-------------+---------------+--------------------+
Contacts table:
+-------------+--------------+--------------------+
| user_id     | contact_name | contact_email      |
+-------------+--------------+--------------------+
| 1           | Bob          | bob@leetcode.com   |
| 1           | John         | john@leetcode.com  |
| 1           | Jal          | jal@leetcode.com   |
| 2           | Omar         | omar@leetcode.com  |
| 2           | Meir         | meir@leetcode.com  |
| 6           | Alice        | alice@leetcode.com |
+-------------+--------------+--------------------+
Invoices table:
+------------+-------+---------+
| invoice_id | price | user_id |
+------------+-------+---------+
| 77         | 100   | 1       |
| 88         | 200   | 1       |
| 99         | 300   | 2       |
| 66         | 400   | 2       |
| 55         | 500   | 13      |
| 44         | 60    | 6       |
+------------+-------+---------+
<strong>Output:</strong> 
+------------+---------------+-------+--------------+----------------------+
| invoice_id | customer_name | price | contacts_cnt | trusted_contacts_cnt |
+------------+---------------+-------+--------------+----------------------+
| 44         | Alex          | 60    | 1            | 1                    |
| 55         | John          | 500   | 0            | 0                    |
| 66         | Bob           | 400   | 2            | 0                    |
| 77         | Alice         | 100   | 3            | 2                    |
| 88         | Alice         | 200   | 3            | 2                    |
| 99         | Bob           | 300   | 2            | 0                    |
+------------+---------------+-------+--------------+----------------------+
<strong>Giải thích:</strong> 
Alice có ba liên hệ, trong đó hai người là liên hệ đáng tin cậy (Bob và John).
Bob có hai liên hệ, nhưng không ai trong số đó là liên hệ đáng tin cậy.
Alex có một liên hệ và đó là liên hệ đáng tin cậy (Alice).
John không có liên hệ nào.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi hóa đơn, ta cần tên khách hàng, số liên hệ và số email trong các liên hệ đó có trong bảng khách hàng. LEFT JOIN hóa đơn với bảng khách hàng để lấy tên, với bảng liên hệ để lấy từng email, rồi JOIN thêm lần nữa với bảng khách hàng theo email đó; nếu khớp thì đó là liên hệ đáng tin cậy. Khi GROUP BY theo hóa đơn, $\mathrm{COUNT}$ bỏ qua giá trị null nên hai số lượng được đếm riêng một cách tự nhiên.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    invoice_id,
    t2.customer_name,
    price,
    COUNT(t3.user_id) AS contacts_cnt,
    COUNT(t4.email) AS trusted_contacts_cnt
FROM
    Invoices AS t1
    LEFT JOIN Customers AS t2 ON t1.user_id = t2.customer_id
    LEFT JOIN Contacts AS t3 ON t1.user_id = t3.user_id
    LEFT JOIN Customers AS t4 ON t3.contact_email = t4.email
GROUP BY invoice_id
ORDER BY invoice_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
