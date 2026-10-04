---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2701. Consecutive Transactions with Increasing Amounts 🔒](https://leetcode.com/problems/consecutive-transactions-with-increasing-amounts)

[中文文档](/solution/2700-2799/2701.Consecutive%20Transactions%20with%20Increasing%20Amounts/README.md)

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
transaction_id là khóa chính của bảng này.
Mỗi hàng chứa thông tin về các giao dịch, bao gồm cặp (customer_id, transaction_date) duy nhất cùng với customer_id và amount tương ứng.
</pre>

<p>Viết một truy vấn SQL để tìm những khách hàng đã thực hiện các giao dịch liên tiếp với <code>amount</code> tăng dần trong ít nhất ba ngày liên tiếp. Bao gồm <code>customer_id</code>, ngày bắt đầu của khoảng thời gian giao dịch liên tiếp và ngày kết thúc của khoảng thời gian giao dịch liên tiếp. Một khách hàng có thể có nhiều giao dịch liên tiếp.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>customer_id, consecutive_start, consecutive_end</code> <em>theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>&nbsp;
Bảng Transactions:
+----------------+-------------+------------------+--------+
| transaction_id | customer_id | transaction_date | amount |
+----------------+-------------+------------------+--------+
| 1 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 101 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-01 &nbsp; &nbsp; &nbsp; | 100 &nbsp; &nbsp;|
| 2 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 101 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-02 &nbsp; &nbsp; &nbsp; | 150 &nbsp; &nbsp;|
| 3 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 101 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-03 &nbsp; &nbsp; &nbsp; | 200 &nbsp; &nbsp;|
| 4 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 102 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-01 &nbsp; &nbsp; &nbsp; | 50 &nbsp; &nbsp; |
| 5 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 102 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-03 &nbsp; &nbsp; &nbsp; | 100 &nbsp; &nbsp;|
| 6 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 102 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-04 &nbsp; &nbsp; &nbsp; | 200 &nbsp; &nbsp;|
| 7 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 105 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-01 &nbsp; &nbsp; &nbsp; | 100 &nbsp; &nbsp;|
| 8 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 105 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-02 &nbsp; &nbsp; &nbsp; | 150 &nbsp; &nbsp;|
| 9 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 105 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-03 &nbsp; &nbsp; &nbsp; | 200 &nbsp; &nbsp;|
| 10 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | 105 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-04 &nbsp; &nbsp; &nbsp; | 300 &nbsp; &nbsp;|
| 11 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | 105 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-12 &nbsp; &nbsp; &nbsp; | 250 &nbsp; &nbsp;|
| 12 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | 105 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-13 &nbsp; &nbsp; &nbsp; | 260 &nbsp; &nbsp;|
| 13 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | 105 &nbsp; &nbsp; &nbsp; &nbsp; | 2023-05-14 &nbsp; &nbsp; &nbsp; | 270 &nbsp; &nbsp;|
+----------------+-------------+------------------+--------+
<strong>Đầu ra:</strong>&nbsp;
+-------------+-------------------+-----------------+
| customer_id | consecutive_start | consecutive_end |&nbsp;
+-------------+-------------------+-----------------+
| 101 &nbsp; &nbsp; &nbsp; &nbsp; |&nbsp; 2023-05-01 &nbsp; &nbsp; &nbsp; |&nbsp;2023-05-03 &nbsp; &nbsp; &nbsp;|&nbsp;
| 105 &nbsp; &nbsp; &nbsp; &nbsp; |&nbsp; 2023-05-01 &nbsp; &nbsp; &nbsp; |&nbsp;2023-05-04 &nbsp; &nbsp; &nbsp;|
| 105 &nbsp; &nbsp; &nbsp; &nbsp; |&nbsp; 2023-05-12 &nbsp; &nbsp; &nbsp; |&nbsp;2023-05-14 &nbsp; &nbsp; &nbsp;|
+-------------+-------------------+-----------------+
<strong>Giải thích:</strong>&nbsp;
- customer_id 101 có các giao dịch liên tiếp với amount tăng dần từ ngày 1 tháng 5 năm 2023 đến ngày 3 tháng 5 năm 2023
- customer_id 102 không có giao dịch liên tiếp nào trong ít nhất 3 ngày.
- customer_id 105 có hai nhóm giao dịch liên tiếp: từ ngày 1 tháng 5 năm 2023 đến ngày 4 tháng 5 năm 2023, và từ ngày 12 tháng 5 năm 2023 đến ngày 14 tháng 5 năm 2023.
customer_id được sắp xếp theo thứ tự tăng dần.
</pre>

<p>&nbsp;</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi hợp lệ cần có amount tăng dần qua các ngày liên tiếp của cùng một khách hàng và kéo dài ít nhất ba ngày. Có thể sắp xếp rồi duyệt từng khách hàng, nhưng self-join để đánh dấu trường hợp "ngày hôm trước tồn tại với amount nhỏ hơn" sẽ biến các điểm ngắt thành một phép tổng hợp bằng window.
>
> Left-join mỗi hàng với hàng liền trước một ngày có amount nhỏ hơn; nếu không tìm thấy thì bắt đầu một segment mới. Tính prefix sum của các điểm ngắt này theo customer và ngày sẽ tạo ra mã segment $s$. Group theo $(customer\_id, s)$ và chỉ giữ các group có độ dài ít nhất $3$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            t1.*,
            SUM(
                CASE
                    WHEN t2.customer_id IS NULL THEN 1
                    ELSE 0
                END
            ) OVER (ORDER BY customer_id, transaction_date) AS s
        FROM
            Transactions AS t1
            LEFT JOIN Transactions AS t2
                ON t1.customer_id = t2.customer_id
                AND t1.amount > t2.amount
                AND DATEDIFF(t1.transaction_date, t2.transaction_date) = 1
    )
SELECT
    customer_id,
    MIN(transaction_date) AS consecutive_start,
    MAX(transaction_date) AS consecutive_end
FROM T
GROUP BY customer_id, s
HAVING COUNT(1) >= 3
ORDER BY customer_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
