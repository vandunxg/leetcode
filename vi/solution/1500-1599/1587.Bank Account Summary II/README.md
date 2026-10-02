---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1587. Bank Account Summary II](https://leetcode.com/problems/bank-account-summary-ii)

[中文文档](/solution/1500-1599/1587.Bank%20Account%20Summary%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Users</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| account      | int     |
| name         | varchar |
+--------------+---------+
account is the primary key (column with unique values) for this table.
Mỗi hàng của bảng này chứa số tài khoản của một người dùng trong ngân hàng.
There will be no two users having the same name in the table.
</pre>

<p>&nbsp;</p>

<p>Table: <code>Transactions</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| trans_id      | int     |
| account       | int     |
| amount        | int     |
| transacted_on | date    |
+---------------+---------+
trans_id is the primary key (column with unique values) for this table.
Mỗi hàng của bảng này chứa một thay đổi được thực hiện trên các tài khoản.
amount is positive if the user received money and negative if they transferred money.
Tất cả tài khoản đều bắt đầu với số dư bằng 0.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo tên và số dư của những người dùng có số dư lớn hơn <code>10000</code>. Số dư của một tài khoản bằng tổng số tiền của tất cả giao dịch liên quan đến tài khoản đó.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>The result format is in the following example.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Users table:
+------------+--------------+
| account    | name         |
+------------+--------------+
| 900001     | Alice        |
| 900002     | Bob          |
| 900003     | Charlie      |
+------------+--------------+
Transactions table:
+------------+------------+------------+---------------+
| trans_id   | account    | amount     | transacted_on |
+------------+------------+------------+---------------+
| 1          | 900001     | 7000       |  2020-08-01   |
| 2          | 900001     | 7000       |  2020-09-01   |
| 3          | 900001     | -3000      |  2020-09-02   |
| 4          | 900002     | 1000       |  2020-09-12   |
| 5          | 900003     | 6000       |  2020-08-07   |
| 6          | 900003     | 6000       |  2020-09-07   |
| 7          | 900003     | -4000      |  2020-09-11   |
+------------+------------+------------+---------------+
<strong>Đầu ra:</strong> 
+------------+------------+
| name       | balance    |
+------------+------------+
| Alice      | 11000      |
+------------+------------+
<strong>Giải thích:</strong> 
Số dư của Alice là (7000 + 7000 - 3000) = 11000.
Số dư của Bob là 1000.
Số dư của Charlie là (6000 + 6000 - 4000) = 8000.
</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Lời giải 1: Equi-Join + Group By + Sum

<!-- thinking:start -->

> **Tư duy**
>
> List users whose balance exceeds $10000$. Names and transactions live in two tables, so they must be joined on the account before we aggregate.
>
> Sau equi-join, tính tổng $amount$ theo từng tài khoản và giữ lại các nhóm có số dư lớn hơn $10000$, đồng thời lấy ra tên.

<!-- thinking:end -->

Ta có thể dùng equi-join để nối bảng `Users` và `Transactions` theo `account`, sau đó nhóm theo `account` và dùng hàm `SUM` để tính số dư cho từng tài khoản. Cuối cùng, ta lọc những người dùng có số dư nhỏ hơn hoặc bằng $10000$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    name,
    SUM(amount) AS balance
FROM
    Users
    JOIN Transactions USING (account)
GROUP BY account
HAVING balance > 10000;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
