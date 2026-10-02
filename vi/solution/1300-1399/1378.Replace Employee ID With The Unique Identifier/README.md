---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier)

[中文文档](/solution/1300-1399/1378.Replace%20Employee%20ID%20With%20The%20Unique%20Identifier/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Employees</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này chứa id và tên của một nhân viên trong công ty.
</pre>

<p>&nbsp;</p>

<p>Table: <code>EmployeeUNI</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
(id, unique_id) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng của bảng này chứa id và unique id tương ứng của một nhân viên trong công ty.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để hiển thị <strong>unique ID</strong> của mỗi người dùng; nếu người dùng không có unique ID thì chỉ hiển thị <code>null</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Employees:
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
Bảng EmployeeUNI:
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
<strong>Đầu ra:</strong> 
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
<strong>Giải thích:</strong> 
Alice và Bob không có unique ID, nên ta sẽ hiển thị null.
Unique ID của Meir là 2.
Unique ID của Winston là 3.
Unique ID của Jonathan là 1.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ghép tên mỗi nhân viên với $\textit{unique\_id}$ của họ, hoặc null nếu không có. LEFT JOIN vẫn giữ lại những nhân viên không có id tương ứng, để $\textit{unique\_id}$ nhận giá trị null.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT unique_id, name
FROM
    Employees
    LEFT JOIN EmployeeUNI USING (id);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
