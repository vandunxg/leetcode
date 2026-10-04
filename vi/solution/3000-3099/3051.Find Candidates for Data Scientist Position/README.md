---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3051. Find Candidates for Data Scientist Position 🔒](https://leetcode.com/problems/find-candidates-for-data-scientist-position)

[中文文档](/solution/3000-3099/3051.Find%20Candidates%20for%20Data%20Scientist%20Position/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace"><code>Candidates</code></font></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| candidate_id | int     |
| skill        | varchar |
+--------------+---------+
(candidate_id, skill) là khóa chính (các cột có giá trị duy nhất) của bảng này.
Mỗi hàng gồm candidate_id và skill.
</pre>

<p>Hãy viết một truy vấn để tìm các <strong>ứng viên</strong> phù hợp nhất cho vị trí Data Scientist. Ứng viên phải thành thạo <strong>Python</strong>, <strong>Tableau</strong> và <strong>PostgreSQL</strong>.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo </em><code>candidate_id</code> <em>theo <strong>thứ tự tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Candidates:
+---------------+--------------+
| candidate_id  | skill        |
+---------------+--------------+
| 123           | Python       |
| 234           | R            |
| 123           | Tableau      |
| 123           | PostgreSQL   |
| 234           | PowerBI      |
| 234           | SQL Server   |
| 147           | Python       |
| 147           | Tableau      |
| 147           | Java         |
| 147           | PostgreSQL   |
| 256           | Tableau      |
| 102           | DataAnalysis |
+---------------+--------------+
<strong>Đầu ra:</strong>
+--------------+
| candidate_id  |
+--------------+
| 123          |
| 147          |
+--------------+
<strong>Giải thích:</strong>
- Các ứng viên 123 và 147 có đủ các kỹ năng cần thiết là Python, Tableau và PostgreSQL cho vị trí data scientist.
- Các ứng viên 234 và 102 không có bất kỳ kỹ năng bắt buộc nào cho vị trí này.
- Ứng viên 256 thành thạo Tableau nhưng thiếu Python và PostgreSQL.
Bảng kết quả được sắp xếp theo candidate_id theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lọc có điều kiện + Thống kê theo nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Một ứng viên phải có Python, Tableau và PostgreSQL. Các kỹ năng nằm trên những hàng riêng biệt, nên không thể dùng ba điều kiện bằng nhau trên cùng một hàng.
>
> Lọc ra ba kỹ năng này, nhóm theo ứng viên và giữ lại những nhóm có đúng $3$ kỹ năng.
>
> Sau khi nhóm, dùng $\texttt{HAVING COUNT}=3$, rồi sắp xếp theo id.

<!-- thinking:end -->

Đầu tiên, chúng ta lọc ra các ứng viên có các kỹ năng `Python`, `Tableau` và `PostgreSQL`. Sau đó, chúng ta nhóm theo `candidate_id` và đếm số kỹ năng của mỗi ứng viên. Cuối cùng, chúng ta lọc các ứng viên có đủ ba kỹ năng này và sắp xếp theo `candidate_id` theo thứ tự tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT candidate_id
FROM Candidates
WHERE skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY 1
HAVING COUNT(1) = 3
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
