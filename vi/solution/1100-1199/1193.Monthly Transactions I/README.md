---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i)

[中文文档](/solution/1100-1199/1193.Monthly%20Transactions%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Transactions</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| country       | varchar |
| state         | enum    |
| amount        | int     |
| trans_date    | date    |
+---------------+---------+
id là khóa chính của bảng này.
Bảng này lưu thông tin về các giao dịch phát sinh.
Cột state là enum với các giá trị [&quot;approved&quot;, &quot;declined&quot;].
</pre>

<p>&nbsp;</p>

<p>Hãy viết truy vấn SQL để tìm theo từng tháng và quốc gia: số lượng giao dịch cùng tổng số tiền, số lượng giao dịch được duyệt cùng tổng số tiền tương ứng.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Bảng Transactions:
+------+---------+----------+--------+------------+
| id   | country | state    | amount | trans_date |
+------+---------+----------+--------+------------+
| 121  | US      | approved | 1000   | 2018-12-18 |
| 122  | US      | declined | 2000   | 2018-12-19 |
| 123  | US      | approved | 2000   | 2019-01-01 |
| 124  | DE      | approved | 2000   | 2019-01-07 |
+------+---------+----------+--------+------------+
<strong>Output:</strong> 
+----------+---------+-------------+----------------+--------------------+-----------------------+
| month    | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
+----------+---------+-------------+----------------+--------------------+-----------------------+
| 2018-12  | US      | 2           | 1              | 3000               | 1000                  |
| 2019-01  | US      | 1           | 1              | 2000               | 2000                  |
| 2019-01  | DE      | 1           | 1              | 2000               | 2000                  |
+----------+---------+-------------+----------------+--------------------+-----------------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Tổng hợp số lượng và số tiền theo năm-tháng và quốc gia. `DATE_FORMAT` lấy tháng; `GROUP BY` theo hai khóa đó, sau đó dùng `COUNT` và `SUM` có điều kiện để tính trên toàn bộ giao dịch và riêng các giao dịch `approved`.

<!-- thinking:end -->

Trước tiên, nhóm theo tháng và quốc gia, sau đó dùng các hàm `COUNT` và `SUM` để lần lượt tính số giao dịch, số giao dịch được duyệt, tổng số tiền và tổng số tiền của các giao dịch được duyệt trong từng nhóm.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(1) AS trans_count,
    SUM(state = 'approved') AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(IF(state = 'approved', amount, 0)) AS approved_total_amount
FROM Transactions
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
