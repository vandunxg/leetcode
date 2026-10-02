---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest)

[中文文档](/solution/1600-1699/1633.Percentage%20of%20Users%20Attended%20a%20Contest/README.md)

## Mô tả

<!-- description:start -->

<p>Bàn:<code>Users</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| user_name   | varchar |
+-------------+---------+
user_id is the primary key (column with unique values) for this table.
Each row of this table contains the name and the id of a user.
</pre>

<p>&nbsp;</p>

<p>Bàn:<code>Register</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| contest_id  | int     |
| user_id     | int     |
+-------------+---------+
(contest_id, user_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id of a user and the contest they registered into.
</pre>

<p>&nbsp;</p>

<p>Viết giải pháp tìm phần trăm số người dùng đã đăng ký trong mỗi cuộc thi được làm tròn thành<strong>two decimals</strong>.</p>

<p>Trả về bảng kết quả theo thứ tự<code>percentage</code>TRONG<strong>descending order</strong>. Trong trường hợp hòa, hãy đặt hàng theo<code>contest_id</code>TRONG<strong>ascending order</strong>.</p>

<p>Định dạng kết quả là trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Users table:
+---------+-----------+
| user_id | user_name |
+---------+-----------+
| 6       | Alice     |
| 2       | Bob       |
| 7       | Alex      |
+---------+-----------+
Register table:
+------------+---------+
| contest_id | user_id |
+------------+---------+
| 215        | 6       |
| 209        | 2       |
| 208        | 2       |
| 210        | 6       |
| 208        | 6       |
| 209        | 7       |
| 209        | 6       |
| 215        | 7       |
| 208        | 7       |
| 210        | 2       |
| 207        | 2       |
| 210        | 7       |
+------------+---------+
<strong>Output:</strong> 
+------------+------------+
| contest_id | percentage |
+------------+------------+
| 208        | 100.0      |
| 209        | 100.0      |
| 210        | 100.0      |
| 215        | 66.67      |
| 207        | 33.33      |
+------------+------------+
<strong>Explanation:</strong> 
All the users registered in contests 208, 209, and 210. The percentage is 100% and we sort them in the answer table by contest_id in ascending order.
Alice and Alex registered in contest 215 and the percentage is ((2/3) * 100) = 66.67%
Bob registered in contest 207 and the percentage is ((1/3) * 100) = 33.33%
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Grouping and Subquery

<!-- thinking:start -->

> **Suy nghĩ**
>
> Tỷ lệ phần trăm số người đăng ký một cuộc thi trên số lượng người dùng. Mẫu số không phụ thuộc vào nhóm, vì vậy một truy vấn con vô hướng là đủ.
>
> Nhóm$\texttt{Register}$qua$\texttt{contest\_id}$, chia cho$(\texttt{SELECT COUNT}(1)\ \texttt{FROM Users})$, làm tròn đến hai số thập phân và sắp xếp theo tỷ lệ phần trăm giảm dần rồi id tăng dần.

<!-- thinking:end -->

Chúng ta có thể nhóm các`Register`bàn theo`contest_id`và đếm số lượng đăng ký cho mỗi cuộc thi. Tỷ lệ đăng ký của mỗi cuộc thi là số lượng đăng ký chia cho tổng số đăng ký.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    contest_id,
    ROUND(COUNT(1) * 100 / (SELECT COUNT(1) FROM Users), 2) AS percentage
FROM Register
GROUP BY 1
ORDER BY 2 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
