---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher)

[中文文档](/solution/2300-2399/2356.Number%20of%20Unique%20Subjects%20Taught%20by%20Each%20Teacher/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Teacher</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| teacher_id  | int  |
| subject_id  | int  |
| dept_id     | int  |
+-------------+------+
(subject_id, dept_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này cho biết giáo viên có teacher_id giảng dạy môn subject_id tại khoa dept_id.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tính số lượng môn học khác nhau mà mỗi giáo viên giảng dạy tại trường đại học.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Teacher:
+------------+------------+---------+
| teacher_id | subject_id | dept_id |
+------------+------------+---------+
| 1          | 2          | 3       |
| 1          | 2          | 4       |
| 1          | 3          | 3       |
| 2          | 1          | 1       |
| 2          | 2          | 1       |
| 2          | 3          | 1       |
| 2          | 4          | 1       |
+------------+------------+---------+
<strong>Đầu ra:</strong>
+------------+-----+
| teacher_id | cnt |
+------------+-----+
| 1          | 2   |
| 2          | 4   |
+------------+-----+
<strong>Giải thích:</strong>
Giáo viên 1:
  - Họ dạy môn 2 ở các khoa 3 và 4.
  - Họ dạy môn 3 ở khoa 3.
Giáo viên 2:
  - Họ dạy môn 1 ở khoa 1.
  - Họ dạy môn 2 ở khoa 1.
  - Họ dạy môn 3 ở khoa 1.
  - Họ dạy môn 4 ở khoa 1.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số môn học khác nhau của từng giáo viên. Cùng một môn học có thể xuất hiện ở nhiều khoa và cần được tính một lần.
>
> Nhóm theo $teacher\_id$ và dùng $COUNT(DISTINCT subject\_id)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
