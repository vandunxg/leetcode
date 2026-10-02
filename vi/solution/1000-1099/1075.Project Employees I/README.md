---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1075. Project Employees I](https://leetcode.com/problems/project-employees-i)

[中文文档](/solution/1000-1099/1075.Project%20Employees%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Project</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+
(project_id, employee_id) là khóa chính của bảng này.
employee_id là khóa ngoại tham chiếu đến bảng <code>Employee</code>.
Mỗi hàng cho biết nhân viên có employee_id đang tham gia dự án có project_id tương ứng.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Employee</code></p>

<pre>
+------------------+---------+
| Tên cột          | Kiểu    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+
employee_id là khóa chính của bảng này. Đảm bảo experience_years không phải NULL.
Mỗi hàng chứa thông tin của một nhân viên.
</pre>

<p>&nbsp;</p>

<p>Viết truy vấn SQL trả về số năm kinh nghiệm <strong>trung bình</strong> của tất cả nhân viên trong từng dự án, <strong>làm tròn đến 2 chữ số thập phân</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả truy vấn như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Project:
+-------------+-------------+
| project_id  | employee_id |
+-------------+-------------+
| 1           | 1           |
| 1           | 2           |
| 1           | 3           |
| 2           | 1           |
| 2           | 4           |
+-------------+-------------+
Bảng Employee:
+-------------+--------+------------------+
| employee_id | name   | experience_years |
+-------------+--------+------------------+
| 1           | Khaled | 3                |
| 2           | Ali    | 2                |
| 3           | John   | 1                |
| 4           | Doe    | 2                |
+-------------+--------+------------------+
<strong>Đầu ra:</strong> 
+-------------+---------------+
| project_id  | average_years |
+-------------+---------------+
| 1           | 2.00          |
| 2           | 2.50          |
+-------------+---------------+
<strong>Giải thích:</strong> Số năm kinh nghiệm trung bình của dự án thứ nhất là (3 + 2 + 1) / 3 = 2.00; của dự án thứ hai là (3 + 2) / 2 = 2.50
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Số năm kinh nghiệm trung bình của mỗi dự án được tính từ `experience_years` của các nhân viên tham gia. Ghép hai bảng theo `employee_id`.
>
> Sau khi inner join, nhóm theo `project_id`, rồi tính trung bình số năm kinh nghiệm và làm tròn đến hai chữ số thập phân.
>
> `ROUND(AVG(...), 2)` tạo kết quả đúng định dạng yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement
SELECT project_id, ROUND(AVG(experience_years), 2) AS average_years
FROM
    Project
    JOIN Employee USING (employee_id)
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
