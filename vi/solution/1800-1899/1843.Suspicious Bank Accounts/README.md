---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1843. Suspicious Bank Accounts 🔒](https://leetcode.com/problems/suspicious-bank-accounts)

[中文文档](/solution/1800-1899/1843.Suspicious%20Bank%20Accounts/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Accounts</code></p>

<pre>
+----------------+------+
| Column Name    | Type |
+----------------+------+
| account_id     | int  |
| max_income     | int  |
+----------------+------+
account_id là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng chứa thông tin về thu nhập tối đa mỗi tháng của một tài khoản ngân hàng.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Transactions</code></p>

<pre>
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| transaction_id | int      |
| account_id     | int      |
| type           | ENUM     |
| amount         | int      |
| day            | datetime |
+----------------+----------+
transaction_id là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng chứa thông tin về một giao dịch.
type là kiểu ENUM (phân loại) gồm (&#39;Creditor&#39;,&#39;Debtor&#39;), trong đó &#39;Creditor&#39; nghĩa là người dùng nộp tiền vào tài khoản và &#39;Debtor&#39; nghĩa là người dùng rút tiền khỏi tài khoản.
amount là số tiền được nộp/rút trong giao dịch.
</pre>

<p>&nbsp;</p>

<p>Một tài khoản ngân hàng được xem là <strong>đáng ngờ</strong> nếu <strong>tổng thu nhập</strong> vượt quá <code>max_income</code> của tài khoản đó trong <strong>hai tháng liên tiếp trở lên</strong>. <strong>Tổng thu nhập</strong> của một tài khoản trong một tháng là tổng tất cả khoản tiền gửi trong tháng đó (tức là các giao dịch có kiểu <code>&#39;Creditor&#39;</code>).</p>

<p>Viết lời giải để báo cáo ID của tất cả tài khoản ngân hàng <strong>đáng ngờ</strong>.</p>

<p>Trả về bảng kết quả <strong>theo bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Accounts:
+------------+------------+
| account_id | max_income |
+------------+------------+
| 3          | 21000      |
| 4          | 10400      |
+------------+------------+
Bảng Transactions:
+----------------+------------+----------+--------+---------------------+
| transaction_id | account_id | type     | amount | day                 |
+----------------+------------+----------+--------+---------------------+
| 2              | 3          | Creditor | 107100 | 2021-06-02 11:38:14 |
| 4              | 4          | Creditor | 10400  | 2021-06-20 12:39:18 |
| 11             | 4          | Debtor   | 58800  | 2021-07-23 12:41:55 |
| 1              | 4          | Creditor | 49300  | 2021-05-03 16:11:04 |
| 15             | 3          | Debtor   | 75500  | 2021-05-23 14:40:20 |
| 10             | 3          | Creditor | 102100 | 2021-06-15 10:37:16 |
| 14             | 4          | Creditor | 56300  | 2021-07-21 12:12:25 |
| 19             | 4          | Debtor   | 101100 | 2021-05-09 15:21:49 |
| 8              | 3          | Creditor | 64900  | 2021-07-26 15:09:56 |
| 7              | 3          | Creditor | 90900  | 2021-06-14 11:23:07 |
+----------------+------------+----------+--------+---------------------+
<strong>Đầu ra:</strong>
+------------+
| account_id |
+------------+
| 3          |
+------------+
<strong>Giải thích:</strong>
Với tài khoản 3:
- Trong tháng 6-2021, người dùng có thu nhập 107100 + 102100 + 90900 = 300100.
- Trong tháng 7-2021, người dùng có thu nhập 64900.
Ta thấy thu nhập vượt mức thu nhập tối đa 21000 trong hai tháng liên tiếp, nên đưa 3 vào bảng kết quả.

Với tài khoản 4:
- Trong tháng 5-2021, người dùng có thu nhập 49300.
- Trong tháng 6-2021, người dùng có thu nhập 10400.
- Trong tháng 7-2021, người dùng có thu nhập 56300.
Ta thấy thu nhập vượt mức tối đa vào tháng 5 và tháng 7 nhưng không vượt vào tháng 6. Vì tài khoản không vượt mức thu nhập tối đa trong hai tháng liên tiếp, ta không đưa tài khoản này vào bảng kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một tài khoản đáng ngờ nếu tổng tiền gửi trong cả hai tháng liên tiếp đều vượt mức thu nhập tối đa của tài khoản. Ta phải tổng hợp theo tài khoản và tháng, sau đó kiểm tra tính liền kề.
>
> Tính tổng tiền gửi theo cửa sổ cho từng tài khoản và tháng, đánh dấu các tháng vượt $max\_income$, rồi self-join các hàng cách nhau đúng một tháng mà cả hai đều được đánh dấu.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT DISTINCT
            t.account_id,
            DATE_FORMAT(day, '%Y-%m-01') AS day,
            transaction_id AS tx,
            SUM(amount) OVER (
                PARTITION BY account_id, DATE_FORMAT(day, '%Y-%m-01')
            ) > max_income AS marked
        FROM
            Transactions AS t
            LEFT JOIN Accounts AS a ON t.account_id = a.account_id
        WHERE type = 'Creditor'
    )
SELECT DISTINCT s1.account_id
FROM
    S AS s1
    LEFT JOIN S AS s2 ON s1.account_id = s2.account_id AND TIMESTAMPDIFF(Month, s1.day, s2.day) = 1
WHERE s1.marked = 1 AND s2.marked = 1
ORDER BY s1.tx;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đánh dấu các tháng bằng window và join theo khoảng cách một tháng. Ta cũng có thể $\textit{GROUP BY}$ tài khoản và $\textit{YYYYMM}$, giữ lại các tháng vượt mức bằng $\textit{HAVING}$, rồi kiểm tra xem $(account\_id,\textit{next month})$ có còn nằm trong tập đó không.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT
            account_id,
            DATE_FORMAT(day, '%Y%m') AS yearmonth,
            transaction_id AS tx
        FROM
            Transactions
            JOIN Accounts USING (account_id)
        WHERE type = 'Creditor'
        GROUP BY account_id, yearmonth
        HAVING SUM(amount) > AVG(max_income)
    )
SELECT DISTINCT account_id
FROM S
WHERE (account_id, PERIOD_ADD(yearmonth, 1)) IN (SELECT account_id, yearmonth FROM S)
ORDER BY tx;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
