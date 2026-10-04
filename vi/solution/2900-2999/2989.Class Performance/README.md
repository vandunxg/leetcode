---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2989. Class Performance 🔒](https://leetcode.com/problems/class-performance)

[中文文档](/solution/2900-2999/2989.Class%20Performance/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Scores</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| student_name | varchar |
| assignment1  | int     |
| assignment2  | int     |
| assignment3  | int     |
+--------------+---------+
student_id is column of unique values for this table.
This table contains student_id, student_name, assignment1, assignment2, and assignment3.
</pre>

<p>Hãy viết lời giải để tính <strong>độ chênh lệch</strong> giữa <strong>tổng điểm</strong> (tổng của cả <code>3</code> bài tập) <strong>cao nhất</strong> và <strong>thấp nhất</strong> mà các học sinh đạt được.</p>

<p><em>Trả về bảng kết quả theo <strong>bất kỳ</strong> thứ tự nào</em><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong>
Scores table:
+------------+--------------+-------------+-------------+-------------+
| student_id | student_name | assignment1 | assignment2 | assignment3 |
+------------+--------------+-------------+-------------+-------------+
| 309        | Owen         | 88          | 47          | 87          |
| 321        | Claire       | 98          | 95          | 37          |
| 338        | Julian       | 100         | 64          | 43          |
| 423        | Peyton       | 60          | 44          | 47          |
| 896        | David        | 32          | 37          | 50          |
| 235        | Camila       | 31          | 53          | 69          |
+------------+--------------+-------------+-------------+-------------+
<strong>Output</strong>
+---------------------+
| difference_in_score |
+---------------------+
| 111                 |
+---------------------+
<strong>Explanation</strong>
- student_id 309 has a total score of 88 + 47 + 87 = 222.
- student_id 321 has a total score of 98 + 95 + 37 = 230.
- student_id 338 has a total score of 100 + 64 + 43 = 207.
- student_id 423 has a total score of 60 + 44 + 47 = 151.
- student_id 896 has a total score of 32 + 37 + 50 = 119.
- student_id 235 has a total score of 31 + 53 + 69 = 153.
student_id 321 has the highest score of 230, while student_id 896 has the lowest score of 119. Therefore, the difference between them is 111.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Giá trị lớn nhất và nhỏ nhất

<!-- thinking:start -->

> **Tư duy**
>
> Độ chênh lệch là khoảng biến thiên của tổng điểm ba bài tập. Lấy $MAX$ và $MIN$ của cùng một tổng rồi trừ cho nhau là đủ, chỉ cần một lần quét.
>
> Không cần nhóm theo từng học sinh.

<!-- thinking:end -->

Ta có thể sử dụng các hàm `MAX` và `MIN` để lấy tổng lớn nhất và nhỏ nhất của `assignment1`, `assignment2` và `assignment3`. Sau đó, lấy tổng lớn nhất trừ đi tổng nhỏ nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    MAX(assignment1 + assignment2 + assignment3) - MIN(
        assignment1 + assignment2 + assignment3
    ) AS difference_in_score
FROM Scores;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
