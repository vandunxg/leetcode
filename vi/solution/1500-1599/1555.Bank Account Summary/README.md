---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1555. Bank Account Summary 🔒](https://leetcode.com/problems/bank-account-summary)

[中文文档](/solution/1500-1599/1555.Bank%20Account%20Summary/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Users</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| user_id      | int     |
| user_name    | varchar |
| credit       | int     |
+--------------+---------+
user_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi dòng của bảng này chứa thông tin credit hiện tại của một người dùng.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Transactions</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| trans_id      | int     |
| paid_by       | int     |
| paid_to       | int     |
| amount        | int     |
| transacted_on | date    |
+---------------+---------+
trans_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi dòng của bảng này chứa thông tin về một giao dịch trong ngân hàng.
Người dùng có id (paid_by) chuyển tiền cho người dùng có id (paid_to).
</pre>

<p>&nbsp;</p>

<p>Leetcode Bank (LCB) hỗ trợ các coder thực hiện thanh toán ảo. Ngân hàng ghi lại mọi giao dịch trong bảng <em>Transaction</em>. Ta muốn tìm số dư hiện tại của tất cả người dùng và kiểm tra xem họ có vượt hạn mức tín dụng hay không (nếu credit hiện tại nhỏ hơn <code>0</code>).</p>

<p>Hãy viết một lời giải&nbsp;để báo cáo.</p>

<ul>
	<li><code>user_id</code>,</li>
	<li><code>user_name</code>,</li>
	<li><code>credit</code>, số dư hiện tại sau khi thực hiện giao dịch, và</li>
	<li><code>credit_limit_breached</code>, kiểm tra credit_limit (<code>&quot;Yes&quot;</code> hoặc <code>&quot;No&quot;</code>)</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Users:
+------------+--------------+-------------+
| user_id    | user_name    | credit      |
+------------+--------------+-------------+
| 1          | Moustafa     | 100         |
| 2          | Jonathan     | 200         |
| 3          | Winston      | 10000       |
| 4          | Luis         | 800         | 
+------------+--------------+-------------+
Bảng Transactions:
+------------+------------+------------+----------+---------------+
| trans_id   | paid_by    | paid_to    | amount   | transacted_on |
+------------+------------+------------+----------+---------------+
| 1          | 1          | 3          | 400      | 2020-08-01    |
| 2          | 3          | 2          | 500      | 2020-08-02    |
| 3          | 2          | 1          | 200      | 2020-08-03    |
+------------+------------+------------+----------+---------------+
<strong>Đầu ra:</strong> 
+------------+------------+------------+-----------------------+
| user_id    | user_name  | credit     | credit_limit_breached |
+------------+------------+------------+-----------------------+
| 1          | Moustafa   | -100       | Yes                   | 
| 2          | Jonathan   | 500        | No                    |
| 3          | Winston    | 9900       | No                    |
| 4          | Luis       | 800        | No                    |
+------------+------------+------------+-----------------------+
<strong>Giải thích:</strong> 
Moustafa paid $400 on &quot;2020-08-01&quot; and received $200 on &quot;2020-08-03&quot;, credit (100 -400 +200) = -$100
Jonathan received $500 on &quot;2020-08-02&quot; and paid $200 on &quot;2020-08-08&quot;, credit (200 +500 -200) = $500
Winston received $400 on &quot;2020-08-01&quot; and paid $500 on &quot;2020-08-03&quot;, credit (10000 +400 -500) = $9990
Luis không nhận khoản chuyển tiền nào, credit = $800
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Báo cáo credit cuối cùng của mỗi người dùng và việc họ có vượt hạn mức hay không. Credit thay đổi qua các khoản chuyển đi, nhận vào và số dư ban đầu; xử lý ba nhóm cập nhật riêng biệt dễ bỏ sót người dùng không có giao dịch.
>
> Ghi các khoản thanh toán là số âm, khoản nhận là số dương, rồi $UNION\ ALL$ chúng với credit ban đầu của từng người dùng. Group theo người dùng sẽ cho số dư cuối, đồng thời người dùng không hoạt động vẫn giữ được dòng ban đầu. Dấu của tổng cho biết hạn mức có bị vượt hay không.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    t.user_id,
    user_name,
    SUM(t.credit) AS credit,
    IF(SUM(t.credit) < 0, 'Yes', 'No') AS credit_limit_breached
FROM
    (
        SELECT paid_by AS user_id, -amount AS credit FROM Transactions
        UNION ALL
        SELECT paid_to AS user_id, amount AS credit FROM Transactions
        UNION ALL
        SELECT user_id, credit FROM Users
    ) AS t
    JOIN Users AS u ON t.user_id = u.user_id
GROUP BY t.user_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
