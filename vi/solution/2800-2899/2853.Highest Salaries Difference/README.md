---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2853. Highest Salaries Difference 🔒](https://leetcode.com/problems/highest-salaries-difference)

[中文文档](/solution/2800-2899/2853.Highest%20Salaries%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Salaries</font></code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| emp_name    | varchar |
| department  | varchar |
| salary      | int     |
+-------------+---------+
(emp_name, department) là khóa chính (tổ hợp các giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này chứa emp_name, department và salary. Sẽ có <strong>ít nhất một</strong> bản ghi thuộc các department engineering và marketing.
</pre>

<p>Hãy viết một lời giải để tính chênh lệch giữa mức lương <strong>cao nhất</strong> trong hai <code>department</code> <strong>marketing</strong> và <strong>engineering</strong>. Trả về giá trị tuyệt đối của chênh lệch mức lương.</p>

<p><em>Trả về bảng kết quả.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Salaries:
+----------+-------------+--------+
| emp_name | department  | salary |
+----------+-------------+--------+
| Kathy    | Engineering | 50000  |
| Roy      | Marketing   | 30000  |
| Charles  | Engineering | 45000  |
| Jack     | Engineering | 85000  |
| Benjamin | Marketing   | 34000  |
| Anthony  | Marketing   | 42000  |
| Edward   | Engineering | 102000 |
| Terry    | Engineering | 44000  |
| Evelyn   | Marketing   | 53000  |
| Arthur   | Engineering | 32000  |
+----------+-------------+--------+
<strong>Đầu ra:</strong>
+-------------------+
| salary_difference |
+-------------------+
| 49000             |
+-------------------+
<strong>Giải thích:</strong>
- Department Engineering và Marketing có mức lương cao nhất lần lượt là 102.000 và 53.000. Do đó, chênh lệch tuyệt đối là 49.000.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mệnh đề GROUP BY

<!-- thinking:start -->

> **Tư duy**
>
> Kết quả là chênh lệch giữa mức lương cao nhất của hai department. Nhóm theo department, lấy `MAX(salary)`, sau đó trừ giá trị nhỏ hơn trong hai mức lương cao nhất khỏi giá trị lớn hơn.

<!-- thinking:end -->

Trước tiên, ta có thể tính mức lương cao nhất của từng department, sau đó tính chênh lệch giữa hai mức lương cao nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT MAX(s) - MIN(s) AS salary_difference
FROM
    (
        SELECT MAX(salary) AS s
        FROM Salaries
        GROUP BY department
    ) AS t;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
