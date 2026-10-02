---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [580. Count Student Number in Departments 🔒](https://leetcode.com/problems/count-student-number-in-departments)

[中文文档](/solution/0500-0599/0580.Count%20Student%20Number%20in%20Departments/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Student</code></p>

<pre>
+--------------+---------+
| Tên cột     | Kiểu dữ liệu |
+--------------+---------+
| student_id   | int     |
| student_name | varchar |
| gender       | varchar |
| dept_id      | int     |
+--------------+---------+
student_id là khóa chính (cột có giá trị duy nhất) của bảng này.
dept_id là khóa ngoại (cột tham chiếu) đến dept_id trong bảng Department.
Mỗi hàng trong bảng này chứa tên, giới tính và mã khoa của một sinh viên.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Department</code></p>

<pre>
+-------------+---------+
| Tên cột    | Kiểu dữ liệu |
+-------------+---------+
| dept_id     | int     |
| dept_name   | varchar |
+-------------+---------+
dept_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này chứa mã và tên của một khoa.
</pre>

<p>&nbsp;</p>

<p>Hãy viết truy vấn trả về tên từng khoa và số sinh viên theo học khoa đó, bao gồm tất cả khoa trong bảng <code>Department</code> (kể cả khoa hiện không có sinh viên).</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>student_number</code> <strong>giảm dần</strong>. Nếu bằng nhau, sắp xếp theo <code>dept_name</code> <strong>theo thứ tự chữ cái</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Student:
+------------+--------------+--------+---------+
| student_id | student_name | gender | dept_id |
+------------+--------------+--------+---------+
| 1          | Jack         | M      | 1       |
| 2          | Jane         | F      | 1       |
| 3          | Mark         | M      | 2       |
+------------+--------------+--------+---------+
Bảng Department:
+---------+-------------+
| dept_id | dept_name   |
+---------+-------------+
| 1       | Engineering |
| 2       | Science     |
| 3       | Law         |
+---------+-------------+
<strong>Đầu ra:</strong> 
+-------------+----------------+
| dept_name   | student_number |
+-------------+----------------+
| Engineering | 2              |
| Science     | 1              |
| Law         | 0              |
+-------------+----------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Left join + Group by

<!-- thinking:start -->

> **Tư duy**
>
> Cần đếm số sinh viên ở từng khoa, kể cả những khoa có số lượng bằng 0. Dùng inner join sẽ loại bỏ các khoa không có sinh viên.
>
> Left join `Student` với `Department`, dùng `COUNT` để đếm theo từng khoa, rồi sắp xếp theo số lượng giảm dần và tên khoa. Left join giữ lại các khoa trống với số lượng bằng $0$.

<!-- thinking:end -->

Ta có thể left join bảng `Department` với bảng `Student` theo `dept_id`, sau đó group by `dept_id` để đếm số sinh viên trong mỗi khoa. Cuối cùng, sắp xếp kết quả theo `student_number` giảm dần rồi theo `dept_name` tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT dept_name, COUNT(student_id) AS student_number
FROM
    Department
    LEFT JOIN Student USING (dept_id)
GROUP BY dept_id
ORDER BY 2 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
