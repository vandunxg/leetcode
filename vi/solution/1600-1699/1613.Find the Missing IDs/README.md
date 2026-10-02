---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1613. Find the Missing IDs 🔒](https://leetcode.com/problems/find-the-missing-ids)

[中文文档](/solution/1600-1699/1613.Find%20the%20Missing%20IDs/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Customers</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| customer_name | varchar |
+---------------+---------+
customer_id is the column with unique values for this table.
Mỗi hàng của bảng này chứa tên và ID của khách hàng.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm các ID khách hàng còn thiếu. ID còn thiếu là những ID không có trong bảng <code>Customers</code> nhưng nằm trong khoảng từ <code>1</code> đến <code>customer_id</code> <strong>lớn nhất</strong> có trong bảng.</p>

<p><strong>Lưu ý</strong> rằng <code>customer_id</code> lớn nhất không vượt quá <code>100</code>.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>ids</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Customers table:
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 1           | Alice         |
| 4           | Bob           |
| 5           | Charlie       |
+-------------+---------------+
<strong>Output:</strong> 
+-----+
| ids |
+-----+
| 2   |
| 3   |
+-----+
<strong>Giải thích:</strong> 
customer_id lớn nhất trong bảng là 5, nên trong khoảng [1,5], ID 2 và 3 bị thiếu.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> ID còn thiếu là các số nguyên dương nhỏ hơn $\texttt{customer\_id}$ lớn nhất nhưng không xuất hiện trong bảng. SQL không có sẵn danh sách số nguyên liên tiếp, nên ta phải tự tạo tập ứng viên.
>
> ID nhiều nhất là $100$, nên một CTE đệ quy tạo các số từ $1$ đến $100$; sau đó loại các ID đã tồn tại và giữ lại những ID nhỏ hơn giá trị lớn nhất.
>
> Truy vấn bên ngoài trả về các $n < \texttt{MAX}(\texttt{customer\_id})$ và $n \texttt{ NOT IN }$ bảng, với tên cột là $\texttt{ids}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH RECURSIVE
    t AS (
        SELECT
            1 AS n
        UNION ALL
        SELECT
            n + 1
        FROM t
        WHERE n < 100
    )
SELECT
    n AS ids
FROM t
WHERE
    n < (
        SELECT
            MAX(customer_id)
        FROM Customers
    )
    AND n NOT IN (
        SELECT
            customer_id
        FROM Customers
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
