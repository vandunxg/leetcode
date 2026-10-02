---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1205. Monthly Transactions II 🔒](https://leetcode.com/problems/monthly-transactions-ii)

[中文文档](/solution/1200-1299/1205.Monthly%20Transactions%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Transactions</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| id             | int     |
| country        | varchar |
| state          | enum    |
| amount         | int     |
| trans_date     | date    |
+----------------+---------+
id là cột có giá trị duy nhất trong bảng này.
Bảng chứa thông tin về các giao dịch đến.
Cột state là ENUM (danh mục) có kiểu [&quot;approved&quot;, &quot;declined&quot;].
</pre>

<p>Bảng: <code>Chargebacks</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| trans_id       | int     |
| trans_date     | date    |
+----------------+---------+
Chargebacks chứa thông tin cơ bản về các chargeback phát sinh từ một số giao dịch trong bảng Transactions.
trans_id là khóa ngoại (cột tham chiếu) đến cột id trong bảng Transactions.
Mỗi chargeback tương ứng với một giao dịch đã thực hiện trước đó, kể cả khi giao dịch đó chưa được duyệt.</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm theo từng tháng và quốc gia: số giao dịch được duyệt cùng tổng số tiền, số chargeback cùng tổng số tiền.</p>

<p><strong>Lưu ý</strong>: Trong lời giải, với mỗi tháng và quốc gia, bỏ qua các hàng có toàn giá trị bằng 0.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Transactions:
+-----+---------+----------+--------+------------+
| id  | country | state    | amount | trans_date |
+-----+---------+----------+--------+------------+
| 101 | US      | approved | 1000   | 2019-05-18 |
| 102 | US      | declined | 2000   | 2019-05-19 |
| 103 | US      | approved | 3000   | 2019-06-10 |
| 104 | US      | declined | 4000   | 2019-06-13 |
| 105 | US      | approved | 5000   | 2019-06-15 |
+-----+---------+----------+--------+------------+
Bảng Chargebacks:
+----------+------------+
| trans_id | trans_date |
+----------+------------+
| 102      | 2019-05-29 |
| 101      | 2019-06-30 |
| 105      | 2019-09-18 |
+----------+------------+
<strong>Đầu ra:</strong> 
+---------+---------+----------------+-----------------+------------------+-------------------+
| month   | country | approved_count | approved_amount | chargeback_count | chargeback_amount |
+---------+---------+----------------+-----------------+------------------+-------------------+
| 2019-05 | US      | 1              | 1000            | 1                | 2000              |
| 2019-06 | US      | 2              | 8000            | 1                | 1000              |
| 2019-09 | US      | 0              | 0               | 1                | 5000              |
+---------+---------+----------------+-----------------+------------------+-------------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Báo cáo theo tháng cần đếm các giao dịch được duyệt trong tháng giao dịch và các chargeback trong tháng chargeback phát sinh. Ngày tháng nằm ở các cột khác nhau, nên chỉ nhóm dữ liệu từ bảng gốc sẽ không thuận tiện.
>
> Chuyển mỗi chargeback thành một hàng có cấu trúc như giao dịch (state là chargeback, date là ngày chargeback), rồi union với các hàng ban đầu sẽ đưa mọi sự kiện về cùng cấp $(month, country)$.
>
> Ta $UNION$ bảng sự kiện, tổng hợp số lượng và số tiền giao dịch được duyệt cũng như chargeback theo tháng và quốc gia, rồi bỏ các nhóm có cả hai loại giá trị đều bằng 0.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT * FROM Transactions
        UNION
        SELECT id, country, 'chargeback', amount, c.trans_date
        FROM
            Transactions AS t
            JOIN Chargebacks AS c ON t.id = c.trans_id
    )
SELECT
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    SUM(state = 'approved') AS approved_count,
    SUM(IF(state = 'approved', amount, 0)) AS approved_amount,
    SUM(state = 'chargeback') AS chargeback_count,
    SUM(IF(state = 'chargeback', amount, 0)) AS chargeback_amount
FROM T
GROUP BY 1, 2
HAVING approved_amount OR chargeback_amount;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
