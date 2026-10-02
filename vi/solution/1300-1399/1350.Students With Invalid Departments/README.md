---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1350. Students With Invalid Departments 🔒](https://leetcode.com/problems/students-with-invalid-departments)

[中文文档](/solution/1300-1399/1350.Students%20With%20Invalid%20Departments/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Departments</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
Trong SQL, id là primary key của bảng này.
Bảng lưu thông tin id của từng khoa trong một trường đại học.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Students</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
| department_id | int     |
+---------------+---------+
Trong SQL, id là primary key của bảng này.
Bảng lưu id của mỗi sinh viên trong trường đại học và id khoa mà sinh viên đó theo học.
</pre>

<p>&nbsp;</p>

<p>Tìm id và tên của tất cả sinh viên đang theo học tại các khoa không còn tồn tại.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Departments:
+------+--------------------------+
| id   | name                     |
+------+--------------------------+
| 1    | Electrical Engineering   |
| 7    | Computer Engineering     |
| 13   | Bussiness Administration |
+------+--------------------------+
Bảng Students:
+------+----------+---------------+
| id   | name     | department_id |
+------+----------+---------------+
| 23   | Alice    | 1             |
| 1    | Bob      | 7             |
| 5    | Jennifer | 13            |
| 2    | John     | 14            |
| 4    | Jasmine  | 77            |
| 3    | Steve    | 74            |
| 6    | Luis     | 1             |
| 8    | Jonathan | 7             |
| 7    | Daiana   | 33            |
| 11   | Madelynn | 1             |
+------+----------+---------------+
<strong>Đầu ra:</strong> 
+------+----------+
| id   | name     |
+------+----------+
| 2    | John     |
| 7    | Daiana   |
| 4    | Jasmine  |
| 3    | Steve    |
+------+----------+
<strong>Giải thích:</strong> 
John, Daiana, Steve và Jasmine lần lượt theo học tại các khoa 14, 33, 74 và 77. Các khoa 14, 33, 74 và 77 không tồn tại trong bảng Departments.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Subquery

<!-- thinking:start -->

> **Tư duy**
>
> Tìm sinh viên có $\textit{department\_id}$ không có trong bảng khoa. Subquery liệt kê mọi id khoa; $\mathrm{NOT\ IN}$ giữ lại những sinh viên không khớp.

<!-- thinking:end -->

Ta có thể dùng subquery để tìm tất cả sinh viên không thuộc khoa nào trong bảng `Departments`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT id, name
FROM Students
WHERE department_id NOT IN (SELECT id FROM Departments);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Left Join

<!-- thinking:start -->

> **Tư duy**
>
> $\mathrm{NOT\ IN}$ có thể bị ảnh hưởng bởi key null. Left join bảng sinh viên với bảng khoa rồi giữ các hàng có id khoa null sẽ xác định những trường hợp không khớp mà không cần subquery.

<!-- thinking:end -->

Ta cũng có thể left join bảng `Students` với `Departments` theo điều kiện `Students.department_id = Departments.id`, rồi lọc các sinh viên có `Departments.id` bằng `NULL`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT s.id, s.name
FROM
    Students AS s
    LEFT JOIN Departments AS d ON s.department_id = d.id
WHERE d.id IS NULL;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
