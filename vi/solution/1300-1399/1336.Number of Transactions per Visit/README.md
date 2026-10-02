---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1336. Number of Transactions per Visit 🔒](https://leetcode.com/problems/number-of-transactions-per-visit)

[中文文档](/solution/1300-1399/1336.Number%20of%20Transactions%20per%20Visit/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Visits</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| visit_date    | date    |
+---------------+---------+
(user_id, visit_date) là primary key (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết user_id đã đến ngân hàng vào visit_date.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Transactions</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| user_id          | int     |
| transaction_date | date    |
| amount           | int     |
+------------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Mỗi hàng trong bảng cho biết user_id đã thực hiện một giao dịch với số tiền amount vào transaction_date.
Đảm bảo người dùng đã đến ngân hàng vào transaction_date (tức là bảng Visits có một hàng chứa (user_id, transaction_date)).
</pre>

<p>&nbsp;</p>

<p>Một ngân hàng muốn lập biểu đồ về số giao dịch mà khách thực hiện trong mỗi lần đến ngân hàng và số lượt khách tương ứng có số giao dịch đó.</p>

<p>Viết lời giải để tìm số người đến ngân hàng nhưng không thực hiện giao dịch nào, số người thực hiện một giao dịch, và tương tự cho các số giao dịch khác.</p>

<p>Bảng kết quả gồm hai cột:</p>

<ul>
	<li><code>transactions_count</code> là số giao dịch được thực hiện trong một lần đến ngân hàng.</li>
	<li><code>visits_count</code> là số lượt người dùng tương ứng đã thực hiện <code>transactions_count</code> giao dịch trong một lần đến ngân hàng.</li>
</ul>

<p><code>transactions_count</code> cần bao gồm mọi giá trị từ <code>0</code> đến <code>max(transactions_count)</code> mà có ít nhất một người dùng đạt được.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>transactions_count</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1336.Number%20of%20Transactions%20per%20Visit/images/chart.png" style="width: 500px; height: 299px;" />
<pre>
<strong>Đầu vào:</strong> 
Bảng Visits:
+---------+------------+
| user_id | visit_date |
+---------+------------+
| 1       | 2020-01-01 |
| 2       | 2020-01-02 |
| 12      | 2020-01-01 |
| 19      | 2020-01-03 |
| 1       | 2020-01-02 |
| 2       | 2020-01-03 |
| 1       | 2020-01-04 |
| 7       | 2020-01-11 |
| 9       | 2020-01-25 |
| 8       | 2020-01-28 |
+---------+------------+
Bảng Transactions:
+---------+------------------+--------+
| user_id | transaction_date | amount |
+---------+------------------+--------+
| 1       | 2020-01-02       | 120    |
| 2       | 2020-01-03       | 22     |
| 7       | 2020-01-11       | 232    |
| 1       | 2020-01-04       | 7      |
| 9       | 2020-01-25       | 33     |
| 9       | 2020-01-25       | 66     |
| 8       | 2020-01-28       | 1      |
| 9       | 2020-01-25       | 99     |
+---------+------------------+--------+
<strong>Đầu ra:</strong> 
+--------------------+--------------+
| transactions_count | visits_count |
+--------------------+--------------+
| 0                  | 4            |
| 1                  | 5            |
| 2                  | 0            |
| 3                  | 1            |
+--------------------+--------------+
<strong>Giải thích:</strong> Biểu đồ của ví dụ này được minh họa ở trên.
* Với transactions_count = 0, các lượt đến (1, &quot;2020-01-01&quot;), (2, &quot;2020-01-02&quot;), (12, &quot;2020-01-01&quot;) và (19, &quot;2020-01-03&quot;) không phát sinh giao dịch, nên visits_count = 4.
* Với transactions_count = 1, các lượt đến (2, &quot;2020-01-03&quot;), (7, &quot;2020-01-11&quot;), (8, &quot;2020-01-28&quot;), (1, &quot;2020-01-02&quot;) và (1, &quot;2020-01-04&quot;) phát sinh một giao dịch, nên visits_count = 5.
* Với transactions_count = 2, không có khách nào đến ngân hàng và thực hiện hai giao dịch, nên visits_count = 0.
* Với transactions_count = 3, lượt đến (9, &quot;2020-01-25&quot;) phát sinh ba giao dịch, nên visits_count = 1.
* Với transactions_count &gt;= 4, không có khách nào đến ngân hàng và thực hiện hơn ba giao dịch, vì vậy ta dừng ở transactions_count = 3.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm số lượt đến theo số giao dịch, đồng thời liệt kê mọi số nguyên từ $0$ đến giá trị lớn nhất, kể cả khi một nhóm không có lượt nào. Recursive CTE tạo dãy các số này; sau đó left join lượt đến với số giao dịch theo từng người dùng và ngày (mặc định là $0$), rồi group theo số giao dịch để tính $\textit{visits\_count}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH RECURSIVE
    S AS (
        SELECT 0 AS n
        UNION
        SELECT n + 1
        FROM S
        WHERE
            n < (
                SELECT MAX(cnt)
                FROM
                    (
                        SELECT COUNT(1) AS cnt
                        FROM Transactions
                        GROUP BY user_id, transaction_date
                    ) AS t
            )
    ),
    T AS (
        SELECT v.user_id, visit_date, IFNULL(cnt, 0) AS cnt
        FROM
            Visits AS v
            LEFT JOIN (
                SELECT user_id, transaction_date, COUNT(1) AS cnt
                FROM Transactions
                GROUP BY 1, 2
            ) AS t
                ON v.user_id = t.user_id AND v.visit_date = t.transaction_date
    )
SELECT n AS transactions_count, COUNT(user_id) AS visits_count
FROM
    S AS s
    LEFT JOIN T AS t ON s.n = t.cnt
GROUP BY n
ORDER BY n;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
