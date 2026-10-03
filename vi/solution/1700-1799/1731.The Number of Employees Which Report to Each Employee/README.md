---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1731. The Number of Employees Which Report to Each Employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee)

[中文文档](/solution/1700-1799/1731.The%20Number%20of%20Employees%20Which%20Report%20to%20Each%20Employee/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Employees</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| reports_to  | int      |
| age         | int      |
+-------------+----------+
employee_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa thông tin về nhân viên và ID của quản lý mà họ báo cáo. Một số nhân viên không báo cáo cho ai (reports_to là null).
</pre>

<p>&nbsp;</p>

<p>Trong bài này, một <strong>quản lý</strong> là nhân viên có ít nhất 1 nhân viên khác báo cáo cho mình.</p>

<p>Viết lời giải để liệt kê ID và tên của tất cả <strong>quản lý</strong>, số nhân viên báo cáo <strong>trực tiếp</strong> cho họ, cùng tuổi trung bình của các nhân viên đó được làm tròn đến số nguyên gần nhất.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>employee_id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong>
Bảng Employees:
+-------------+---------+------------+-----+
| employee_id | name    | reports_to | age |
+-------------+---------+------------+-----+
| 9           | Hercy   | null       | 43  |
| 6           | Alice   | 9          | 41  |
| 4           | Bob     | 9          | 36  |
| 2           | Winston | null       | 37  |
+-------------+---------+------------+-----+
<strong>Output:</strong>
+-------------+-------+---------------+-------------+
| employee_id | name  | reports_count | average_age |
+-------------+-------+---------------+-------------+
| 9           | Hercy | 2             | 39          |
+-------------+-------+---------------+-------------+
<strong>Giải thích:</strong> Hercy có 2 người báo cáo trực tiếp là Alice và Bob. Tuổi trung bình của họ là (41+36)/2 = 38.5, làm tròn đến số nguyên gần nhất được 39.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong>
Employees table:
+-------------+---------+------------+-----+
| employee_id | name &nbsp; &nbsp;| reports_to | age |
|-------------|---------|------------|-----|
| 1 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Michael | null &nbsp; &nbsp; &nbsp; | 45 &nbsp;|
| 2 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Alice &nbsp; | 1 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 38 &nbsp;|
| 3 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Bob &nbsp; &nbsp; | 1 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 42 &nbsp;|
| 4 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Charlie | 2 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 34 &nbsp;|
| 5 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | David &nbsp; | 2 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 40 &nbsp;|
| 6 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Eve &nbsp; &nbsp; | 3 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;| 37 &nbsp;|
| 7 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Frank &nbsp; | null &nbsp; &nbsp; &nbsp; | 50 &nbsp;|
| 8 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Grace &nbsp; | null &nbsp; &nbsp; &nbsp; | 48 &nbsp;|
+-------------+---------+------------+-----+
<strong>Output:</strong>
+-------------+---------+---------------+-------------+
| employee_id | name &nbsp; &nbsp;| reports_count | average_age |
| ----------- | ------- | ------------- | ----------- |
| 1 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Michael | 2 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | 40 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|
| 2 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Alice &nbsp; | 2 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | 37 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|
| 3 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | Bob &nbsp; &nbsp; | 1 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | 37 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|
+-------------+---------+---------------+-------------+

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Self-Join + Nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhân viên trỏ đến quản lý qua $\textit{reports\_to}$. Ta cần số lượng nhân viên báo cáo và tuổi trung bình của từng quản lý.
>
> Self-join các nhân viên báo cáo với quản lý, sau đó dùng $\mathrm{COUNT}$ và $\mathrm{ROUND}(\mathrm{AVG}(\textit{age}))$, nhóm theo quản lý.

<!-- thinking:end -->

Ta có thể dùng self-join để nối thông tin quản lý cấp trên của mỗi nhân viên với thông tin của nhân viên đó, sau đó nhóm và tổng hợp để đếm số cấp dưới và tính tuổi trung bình của từng quản lý.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    e2.employee_id,
    e2.name,
    COUNT(1) AS reports_count,
    ROUND(AVG(e1.age)) AS average_age
FROM
    Employees AS e1
    JOIN Employees AS e2 ON e1.reports_to = e2.employee_id
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
