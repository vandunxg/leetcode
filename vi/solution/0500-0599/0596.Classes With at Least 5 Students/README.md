---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [596. Classes With at Least 5 Students](https://leetcode.com/problems/classes-with-at-least-5-students)

[中文文档](/solution/0500-0599/0596.Classes%20With%20at%20Least%205%20Students/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Courses</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+
(student, class) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết tên một học sinh và lớp mà học sinh đó đang theo học.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm tất cả các lớp có <strong>ít nhất năm học sinh</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Courses:
+---------+----------+
| student | class    |
+---------+----------+
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |
+---------+----------+
<strong>Đầu ra:</strong> 
+---------+
| class   |
+---------+
| Math    |
+---------+
<strong>Giải thích:</strong> 
- Lớp Math có 6 học sinh nên được đưa vào kết quả.
- Lớp English có 1 học sinh nên không được đưa vào kết quả.
- Lớp Biology có 1 học sinh nên không được đưa vào kết quả.
- Lớp Computer có 1 học sinh nên không được đưa vào kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm các lớp có ít nhất năm học sinh. Nhóm và đếm rồi lọc bằng `HAVING`; không cần subquery.
>
> Dùng `GROUP BY class`, sau đó lọc với `HAVING COUNT(*) >= 5`. `HAVING` chạy sau khi nhóm, khác với `WHERE`.

<!-- thinking:end -->

Ta có thể dùng câu lệnh `GROUP BY` để nhóm theo lớp, sau đó dùng `HAVING` để lọc các lớp có số học sinh lớn hơn hoặc bằng $5$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT class
FROM Courses
GROUP BY 1
HAVING COUNT(1) >= 5;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
