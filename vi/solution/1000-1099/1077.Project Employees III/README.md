---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1077. Project Employees III 🔒](https://leetcode.com/problems/project-employees-iii)

[中文文档](/solution/1000-1099/1077.Project%20Employees%20III/README.md)

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
Mỗi hàng trong bảng này cho biết nhân viên có employee_id đang làm việc trong dự án có project_id tương ứng.
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
Mỗi hàng trong bảng này chứa thông tin về một nhân viên.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm các nhân viên <strong>có nhiều kinh nghiệm nhất</strong> trong từng dự án. Nếu có nhiều người đồng hạng, hãy trả về tất cả nhân viên có số năm kinh nghiệm cao nhất.</p>

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
| 3           | John   | 3                |
| 4           | Doe    | 2                |
+-------------+--------+------------------+
<strong>Đầu ra:</strong> 
+-------------+---------------+
| project_id  | employee_id   |
+-------------+---------------+
| 1           | 1             |
| 1           | 3             |
| 2           | 1             |
+-------------+---------------+
<strong>Giải thích:</strong> Hai nhân viên có id 1 và 3 cùng có nhiều kinh nghiệm nhất trong số nhân viên của dự án thứ nhất. Ở dự án thứ hai, nhân viên có id 1 có nhiều kinh nghiệm nhất.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Inner Join + Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi dự án, cần giữ lại nhân viên có nhiều kinh nghiệm nhất, kể cả trường hợp đồng hạng. Sau khi join, ta so sánh `experience_years` trong từng dự án.
>
> `RANK() OVER (PARTITION BY project_id ORDER BY experience_years DESC)` gán hạng $1$ cho số năm kinh nghiệm cao nhất.
>
> Query bên ngoài trả về các cặp `project_id, employee_id` đó.

<!-- thinking:end -->

Trước tiên, ta inner join hai bảng `Project` và `Employee`, sau đó dùng window function `rank()` để phân nhóm theo dự án và sắp xếp giảm dần theo `experience_years`. Cuối cùng, chọn nhân viên có nhiều kinh nghiệm nhất trong mỗi dự án.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY project_id
                ORDER BY experience_years DESC
            ) AS rk
        FROM
            Project
            JOIN Employee USING (employee_id)
    )
SELECT project_id, employee_id
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
