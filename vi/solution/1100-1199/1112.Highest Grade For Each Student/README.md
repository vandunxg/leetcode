---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1112. Highest Grade For Each Student 🔒](https://leetcode.com/problems/highest-grade-for-each-student)

[中文文档](/solution/1100-1199/1112.Highest%20Grade%20For%20Each%20Student/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Enrollments</code></p>

<pre>
+---------------+---------+
| Tên cột       | Kiểu    |
+---------------+---------+
| student_id    | int     |
| course_id     | int     |
| grade         | int     |
+---------------+---------+
(student_id, course_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
grade không bao giờ là NULL.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm điểm cao nhất và khóa học tương ứng của từng học sinh. Nếu có nhiều khóa học cùng điểm cao nhất, hãy chọn khóa học có <code>course_id</code> nhỏ nhất.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>student_id</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Enrollments:
+------------+-------------------+
| student_id | course_id | grade |
+------------+-----------+-------+
| 2          | 2         | 95    |
| 2          | 3         | 95    |
| 1          | 1         | 90    |
| 1          | 2         | 99    |
| 3          | 1         | 80    |
| 3          | 2         | 75    |
| 3          | 3         | 82    |
+------------+-----------+-------+
<strong>Đầu ra:</strong> 
+------------+-------------------+
| student_id | course_id | grade |
+------------+-----------+-------+
| 1          | 2         | 99    |
| 2          | 2         | 95    |
| 3          | 3         | 82    |
+------------+-----------+-------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ RANK() OVER()

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi học sinh, lấy điểm cao nhất và phá hòa bằng `course_id` nhỏ nhất. `RANK() OVER (PARTITION BY student_id ORDER BY grade DESC, course_id)` biểu diễn thứ tự này trong một lượt; các hàng có rank $1$ là kết quả, sau đó sắp xếp theo `student_id`.

<!-- thinking:end -->

Ta có thể dùng hàm cửa sổ `RANK() OVER()` để sắp xếp điểm của mỗi học sinh theo thứ tự giảm dần. Nếu điểm bằng nhau, sắp xếp theo mã khóa học tăng dần, rồi chọn hàng có rank bằng $1$ của từng học sinh.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY student_id
                ORDER BY grade DESC, course_id
            ) AS rk
        FROM Enrollments
    )
SELECT student_id, course_id, grade
FROM T
WHERE rk = 1
ORDER BY student_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Truy vấn con

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cần hàm cửa sổ. Nếu không dùng hàm này, hãy gom nhóm để lấy `MAX(grade)` của từng học sinh, sau đó lọc các hàng có điểm tương ứng trong bảng gốc và lấy `MIN(course_id)`. Hai phép tổng hợp lần lượt tìm “điểm cao nhất” và “mã khóa học nhỏ nhất khi hòa điểm”.

<!-- thinking:end -->

Trước tiên, truy vấn điểm cao nhất của mỗi học sinh, rồi tìm mã khóa học nhỏ nhất tương ứng với điểm cao nhất đó.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT student_id, MIN(course_id) AS course_id, grade
FROM Enrollments
WHERE
    (student_id, grade) IN (
        SELECT student_id, MAX(grade) AS grade
        FROM Enrollments
        GROUP BY 1
    )
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
