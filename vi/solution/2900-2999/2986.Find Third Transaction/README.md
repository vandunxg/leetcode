---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2986. Find Third Transaction 🔒](https://leetcode.com/problems/find-third-transaction)

[中文文档](/solution/2900-2999/2986.Find%20Third%20Transaction/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Transactions</code></p>

<pre>
+------------------+----------+
| Column Name      | Type     |
+------------------+----------+
| user_id          | int      |
| spend            | decimal  |
| transaction_date | datetime |
+------------------+----------+
(user_id, transaction_date) là cột chứa các giá trị duy nhất trong bảng này.
Bảng này chứa user_id, spend và transaction_date.
</pre>

<p>Hãy viết lời giải để tìm <strong>giao dịch thứ ba</strong> (nếu có ít nhất ba giao dịch) của mỗi người dùng, trong đó <strong>số tiền chi tiêu</strong> ở <strong>hai giao dịch trước đó</strong> <strong>thấp hơn</strong> số tiền chi tiêu ở <strong>giao dịch thứ ba</strong>.</p>

<p><em>Trả về bảng kết quả theo</em> <code>user_id</code> <em>với thứ tự <strong>tăng dần</strong></em><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Transactions table:
+---------+--------+---------------------+
| user_id | spend  | transaction_date    |
+---------+--------+---------------------+
| 1       | 65.56  | 2023-11-18 13:49:42 |
| 1       | 96.0   | 2023-11-30 02:47:26 |
| 1       | 7.44   | 2023-11-02 12:15:23 |
| 1       | 49.78  | 2023-11-12 00:13:46 |
| 2       | 40.89  | 2023-11-21 04:39:15 |
| 2       | 100.44 | 2023-11-20 07:39:34 |
| 3       | 37.33  | 2023-11-03 06:22:02 |
| 3       | 13.89  | 2023-11-11 16:00:14 |
| 3       | 7.0    | 2023-11-29 22:32:36 |
+---------+--------+---------------------+
<strong>Đầu ra</strong>
+---------+-------------------------+------------------------+
| user_id | third_transaction_spend | third_transaction_date |
+---------+-------------------------+------------------------+
| 1       | 65.56                   | 2023-11-18 13:49:42    |
+---------+-------------------------+------------------------+
<strong>Giải thích</strong>
- Với user_id 1, giao dịch thứ ba diễn ra vào ngày 2023-11-18 lúc 13:49:42 với số tiền $65.56, surpassing the expenditures of the previous two transactions which were $7.44 vào ngày 2023-11-02 lúc 12:15:23 và $49.78 vào ngày 2023-11-12 lúc 00:13:46. Vì vậy, giao dịch thứ ba này được đưa vào bảng kết quả.
- user_id 2 chỉ có tổng cộng 2 giao dịch, nên không có giao dịch thứ ba để xét.
- Với user_id 3, số tiền $7.0 của giao dịch thứ ba thấp hơn số tiền của hai giao dịch trước đó, nên giao dịch này không được đưa vào kết quả.
Bảng kết quả được sắp xếp theo user_id theo thứ tự tăng dần.

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi người dùng, ta cần tìm giao dịch thứ ba theo thời gian, có spend lớn hơn nghiêm ngặt hai giao dịch trước đó. $RANK$ đánh số các dòng của từng người dùng; hai lần dùng $LAG$ lấy spend của các giao dịch trước đó và tạo thành giá trị boolean $st$.
>
> Giữ lại $rk=3$ và $st=1$. Không cần self-join ba lần.

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
                PARTITION BY user_id
                ORDER BY transaction_date
            ) AS rk,
            spend > (
                LAG(spend) OVER (
                    PARTITION BY user_id
                    ORDER BY transaction_date
                )
            )
            AND spend > (
                LAG(spend, 2) OVER (
                    PARTITION BY user_id
                    ORDER BY transaction_date
                )
            ) AS st
        FROM Transactions
    )
SELECT user_id, spend AS third_transaction_spend, transaction_date AS third_transaction_date
FROM T
WHERE rk = 3 AND st = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
