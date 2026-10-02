---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions)

[中文文档](/solution/1500-1599/1581.Customer%20Who%20Visited%20but%20Did%20Not%20Make%20Any%20Transactions/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Visits</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+
visit_id is the column with unique values for this table.
Bảng này chứa thông tin về những khách hàng đã ghé trung tâm thương mại.
</pre>

<p>&nbsp;</p>

<p>Table: <code>Transactions</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
transaction_id is column with unique values for this table.
Bảng này chứa thông tin về các giao dịch được thực hiện trong lượt ghé có visit_id.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm ID của những người dùng đã ghé thăm nhưng không thực hiện giao dịch nào, cùng số lần họ có những lượt ghé thăm như vậy.</p>

<p>Trả về bảng kết quả được sắp xếp theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Visits
+----------+-------------+
| visit_id | customer_id |
+----------+-------------+
| 1        | 23          |
| 2        | 9           |
| 4        | 30          |
| 5        | 54          |
| 6        | 96          |
| 7        | 54          |
| 8        | 54          |
+----------+-------------+
Transactions
+----------------+----------+--------+
| transaction_id | visit_id | amount |
+----------------+----------+--------+
| 2              | 5        | 310    |
| 3              | 5        | 300    |
| 9              | 5        | 200    |
| 12             | 1        | 910    |
| 13             | 2        | 970    |
+----------------+----------+--------+
<strong>Đầu ra:</strong> 
+-------------+----------------+
| customer_id | count_no_trans |
+-------------+----------------+
| 54          | 2              |
| 30          | 1              |
| 96          | 1              |
+-------------+----------------+
<strong>Giải thích:</strong> 
Khách hàng có id = 23 ghé trung tâm thương mại một lần và thực hiện một giao dịch trong lượt ghé có id = 12.
Khách hàng có id = 9 ghé trung tâm thương mại một lần và thực hiện một giao dịch trong lượt ghé có id = 13.
Khách hàng có id = 30 ghé trung tâm thương mại một lần nhưng không thực hiện giao dịch nào.
Khách hàng có id = 54 ghé trung tâm thương mại ba lần. Trong 2 lượt họ không thực hiện giao dịch nào, còn trong một lượt họ thực hiện 3 giao dịch.
Khách hàng có id = 96 ghé trung tâm thương mại một lần nhưng không thực hiện giao dịch nào.
Như vậy, người dùng có ID 30 và 96 mỗi người có một lượt ghé không giao dịch. Người dùng 54 có hai lượt ghé không giao dịch.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Subquery + Nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Count, per customer, visits that have no transaction. Purchases live in another table, so we need visits without a matching row.
>
> A subquery lists every $visit\_id$ that appears in $Transactions$; the outer query keeps the complement and groups by customer.

<!-- thinking:end -->

Ta có thể dùng một subquery để tìm trước tất cả `visit_id` không có giao dịch, sau đó nhóm theo `customer_id` để đếm số lần mỗi khách hàng không thực hiện giao dịch.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT customer_id, COUNT(1) AS count_no_trans
FROM Visits
WHERE visit_id NOT IN (SELECT visit_id FROM Transactions)
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Left Join + Nhóm

<!-- thinking:start -->

> **Tư duy**
>
> $NOT\ IN$ is awkward with nulls on some engines. A left join from $Visits$ to $Transactions$ leaves $amount$ null on unmatched visits; filtering those rows and grouping yields the same counts and composes more easily with extra columns.

<!-- thinking:end -->

Ta cũng có thể dùng left join để nối bảng `Visits` và `Transactions` theo `visit_id`, sau đó lọc các bản ghi có `amount` là `NULL`. Cuối cùng, nhóm theo `customer_id` để đếm số lần mỗi khách hàng không thực hiện giao dịch.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT customer_id, COUNT(1) AS count_no_trans
FROM
    Visits
    LEFT JOIN Transactions USING (visit_id)
WHERE amount IS NULL
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
