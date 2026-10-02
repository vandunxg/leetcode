---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1076. Project Employees II 🔒](https://leetcode.com/problems/project-employees-ii)

[中文文档](/solution/1000-1099/1076.Project%20Employees%20II/README.md)

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
(project_id, employee_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
employee_id là khóa ngoại (cột tham chiếu) đến bảng <code>Employee</code>.
Mỗi dòng trong bảng cho biết nhân viên có employee_id đang làm việc trong dự án có project_id tương ứng.
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
employee_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi dòng trong bảng chứa thông tin về một nhân viên.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm tất cả <strong>dự án</strong> có số nhân viên nhiều nhất.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

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
+-------------+
| project_id  |
+-------------+
| 1           |
+-------------+
<strong>Giải thích:</strong> Dự án thứ nhất có 3 nhân viên, còn dự án thứ hai có 2.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nhiều dự án có thể cùng đạt số nhân viên cao nhất. Sau khi đếm nhân viên của từng dự án, giữ lại mọi `project_id` có số lượng không thấp hơn bất kỳ nhóm nào khác.
>
> `HAVING COUNT(1) >= ALL (grouped counts)` chọn tất cả dự án có số lượng lớn nhất.
>
> Subquery chỉ đếm; query bên ngoài dùng kết quả để lọc.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT project_id
FROM Project
GROUP BY 1
HAVING
    COUNT(1) >= all(
        SELECT COUNT(1)
        FROM Project
        GROUP BY project_id
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Subquery `ALL` cần thực hiện phép tổng hợp lần nữa. `RANK()` sắp xếp theo số nhân viên giảm dần và gán hạng $1$ cho mọi dự án đạt mức cao nhất, kể cả khi đồng hạng.
>
> CTE tính `rk`; query bên ngoài giữ lại các dòng có `rk = 1`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            project_id,
            RANK() OVER (ORDER BY COUNT(employee_id) DESC) AS rk
        FROM Project
        GROUP BY 1
    )
SELECT project_id
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
