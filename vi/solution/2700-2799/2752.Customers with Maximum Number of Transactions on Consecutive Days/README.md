---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2752. Customers with Maximum Number of Transactions on Consecutive Days 🔒](https://leetcode.com/problems/customers-with-maximum-number-of-transactions-on-consecutive-days)

[中文文档](/solution/2700-2799/2752.Customers%20with%20Maximum%20Number%20of%20Transactions%20on%20Consecutive%20Days/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Transactions</code></p>

<pre>
+------------------+------+
| Column Name      | Type |
+------------------+------+
| transaction_id   | int  |
| customer_id      | int  |
| transaction_date | date |
| amount           | int  |
+------------------+------+
transaction_id là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng chứa thông tin về một giao dịch, bao gồm cặp (customer_id, transaction_date) duy nhất cùng với customer_id và amount tương ứng.
</pre>

<p>Viết lời giải để tìm tất cả <code>customer_id</code> đã thực hiện số giao dịch lớn nhất trong các ngày liên tiếp.</p>

<p>Trả về tất cả <code>customer_id</code> có số giao dịch liên tiếp lớn nhất.&nbsp;Sắp xếp bảng kết quả theo&nbsp;<code>customer_id</code> theo thứ tự <strong>tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Transactions:
+----------------+-------------+------------------+--------+
| transaction_id | customer_id | transaction_date | amount |
+----------------+-------------+------------------+--------+
| 1              | 101         | 2023-05-01       | 100    |
| 2              | 101         | 2023-05-02       | 150    |
| 3              | 101         | 2023-05-03       | 200    |
| 4              | 102         | 2023-05-01       | 50     |
| 5              | 102         | 2023-05-03       | 100    |
| 6              | 102         | 2023-05-04       | 200    |
| 7              | 105         | 2023-05-01       | 100    |
| 8              | 105         | 2023-05-02       | 150    |
| 9              | 105         | 2023-05-03       | 200    |
+----------------+-------------+------------------+--------+
<strong>Đầu ra:</strong>
+-------------+
| customer_id |
+-------------+
| 101         |
| 105         |
+-------------+
<strong>Giải thích:</strong>
- customer_id 101 có tổng cộng 3 giao dịch và tất cả đều diễn ra trong các ngày liên tiếp.
- customer_id 102 có tổng cộng 3 giao dịch, nhưng chỉ 2 giao dịch trong số đó diễn ra trong các ngày liên tiếp.
- customer_id 105 có tổng cộng 3 giao dịch và tất cả đều diễn ra trong các ngày liên tiếp.
Tổng số giao dịch liên tiếp lớn nhất là 3, đạt được bởi customer_id 101 và 105. Các customer_id được sắp xếp theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi khách hàng, tìm chuỗi ngày giao dịch liên tiếp dài nhất, sau đó giữ lại những khách hàng đạt mức tối đa trên toàn bộ dữ liệu. Việc quét các ngày đã được sắp xếp theo từng khách hàng sẽ hiệu quả; lấy ngày trừ đi số thứ tự của hàng sẽ đưa các ngày trong cùng một chuỗi về cùng một key.
>
> Đánh số các hàng theo từng khách hàng dựa trên ngày, lấy ngày trừ đi chỉ số đó để đánh dấu chuỗi, nhóm theo khách hàng và nhãn, rồi giữ lại những khách hàng có số lượng bằng mức tối đa trên toàn bộ dữ liệu.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    s AS (
        SELECT
            customer_id,
            DATE_SUB(
                transaction_date,
                INTERVAL ROW_NUMBER() OVER (
                    PARTITION BY customer_id
                    ORDER BY transaction_date
                ) DAY
            ) AS transaction_date
        FROM Transactions
    ),
    t AS (
        SELECT customer_id, transaction_date, COUNT(1) AS cnt
        FROM s
        GROUP BY 1, 2
    )
SELECT customer_id
FROM t
WHERE cnt = (SELECT MAX(cnt) FROM t)
ORDER BY customer_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
