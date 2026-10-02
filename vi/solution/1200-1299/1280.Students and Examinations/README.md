---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations)

[中文文档](/solution/1200-1299/1280.Students%20and%20Examinations/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Students</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
student_id là khóa chính của bảng này (cột có giá trị duy nhất).
Mỗi hàng trong bảng chứa ID và tên của một học sinh trong trường.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Subjects</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
subject_name là khóa chính của bảng này (cột có giá trị duy nhất).
Mỗi hàng trong bảng chứa tên của một môn học trong trường.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Examinations</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
Bảng này không có khóa chính (cột có giá trị duy nhất) và có thể chứa các hàng trùng lặp.
Mỗi học sinh trong bảng Students đều tham gia mọi môn học trong bảng Subjects.
Mỗi hàng trong bảng cho biết học sinh có ID student_id đã tham dự kỳ thi môn subject_name.
</pre>

<p>&nbsp;</p>

<p>Viết truy vấn tìm số lần mỗi học sinh tham dự từng kỳ thi.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>student_id</code> và <code>subject_name</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Students:
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |
+------------+--------------+
Bảng Subjects:
+--------------+
| subject_name |
+--------------+
| Math         |
| Physics      |
| Programming  |
+--------------+
Bảng Examinations:
+------------+--------------+
| student_id | subject_name |
+------------+--------------+
| 1          | Math         |
| 1          | Physics      |
| 1          | Programming  |
| 2          | Programming  |
| 1          | Physics      |
| 1          | Math         |
| 13         | Math         |
| 13         | Programming  |
| 13         | Physics      |
| 2          | Math         |
| 1          | Math         |
+------------+--------------+
<strong>Đầu ra:</strong> 
+------------+--------------+--------------+----------------+
| student_id | student_name | subject_name | attended_exams |
+------------+--------------+--------------+----------------+
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |
+------------+--------------+--------------+----------------+
<strong>Giải thích:</strong> 
Bảng kết quả cần chứa tất cả học sinh và tất cả môn học.
Alice đã tham dự kỳ thi Math 3 lần, Physics 2 lần và Programming 1 lần.
Bob đã tham dự kỳ thi Math 1 lần, Programming 1 lần và không tham dự kỳ thi Physics.
Alex không tham dự kỳ thi nào.
John đã tham dự mỗi kỳ thi Math, Physics và Programming 1 lần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai phép Join + Grouping

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm số lần thi của từng học sinh ở từng môn, kể cả trường hợp bằng 0. Tạo tích Descartes giữa học sinh và môn học, left join các hàng thi rồi group theo học sinh và môn. Left join giữ lại những cặp không có lượt thi; $COUNT$ trả về 0 khi không có hàng khớp.

<!-- thinking:end -->

Trước tiên, ta join bảng `Students` với bảng `Subjects` để tạo mọi tổ hợp học sinh và môn học, sau đó join bảng `Examinations` theo `student_id` và `subject_name`. Nhờ đó, ta có thể đếm số lần mỗi học sinh thi từng môn. Cuối cùng, group theo `student_id` và `subject_name` để tính số lượt thi.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT student_id, student_name, subject_name, COUNT(e.student_id) AS attended_exams
FROM
    Students
    JOIN Subjects
    LEFT JOIN Examinations AS e USING (student_id, subject_name)
GROUP BY 1, 3
ORDER BY 1, 3;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
