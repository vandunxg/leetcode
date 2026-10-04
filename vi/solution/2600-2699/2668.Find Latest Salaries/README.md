---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2668. Find Latest Salaries 🔒](https://leetcode.com/problems/find-latest-salaries)

[中文文档](/solution/2600-2699/2668.Find%20Latest%20Salaries/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Salary</font></code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| emp_id        | int     |
| firstname     | varchar |
| lastname      | varchar |
| salary        | varchar |
| department_id | varchar |
+---------------+---------+
(emp_id, salary) là khóa chính (kết hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin nhân viên và mức lương hàng năm của họ, tuy nhiên một số bản ghi đã cũ và chứa thông tin lương không còn cập nhật.
</pre>

<p>Hãy viết lời giải để tìm mức lương hiện tại của mỗi nhân viên, với giả định rằng lương tăng mỗi năm. Kết quả cần chứa <code>emp_id</code>, <code>firstname</code>, <code>lastname</code>, <code>salary</code> và <code>department_id</code> của họ.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>emp_id</code> theo thứ tự <strong>tăng dần</strong><em>.</em></p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>Bảng <code>Salary</code>:
+--------+-----------+----------+--------+---------------+
| emp_id | firstname | lastname | salary | department_id |
+--------+-----------+----------+--------+---------------+
| 1      | Todd      | Wilson   | 110000 | D1006         |
| 1      | Todd      | Wilson   | 106119 | D1006         |
| 2      | Justin    | Simon    | 128922 | D1005         |
| 2      | Justin    | Simon    | 130000 | D1005         |
| 3      | Kelly     | Rosario  | 42689  | D1002         |
| 4      | Patricia  | Powell   | 162825 | D1004         |
| 4      | Patricia  | Powell   | 170000 | D1004         |
| 5      | Sherry    | Golden   | 44101  | D1002         |
| 6      | Natasha   | Swanson  | 79632  | D1005         |
| 6      | Natasha   | Swanson  | 90000  | D1005         |
+--------+-----------+----------+--------+---------------+
<strong>Đầu ra:
</strong>+--------+-----------+----------+--------+---------------+
| emp_id | firstname | lastname | salary | department_id |
+--------+-----------+----------+--------+---------------+
| 1      | Todd      | Wilson   | 110000 | D1006         |
| 2      | Justin    | Simon    | 130000 | D1005         |
| 3      | Kelly     | Rosario  | 42689  | D1002         |
| 4      | Patricia  | Powell   | 170000 | D1004         |
| 5      | Sherry    | Golden   | 44101  | D1002         |
| 6      | Natasha   | Swanson  | 90000  | D1005         |
+--------+-----------+----------+--------+---------------+<strong>
</strong>
<strong>Giải thích:</strong>
- emp_id 1 có hai bản ghi với mức lương&nbsp;110000, 106119; trong đó 110000 là mức lương được cập nhật (giả định lương tăng mỗi năm).
- emp_id 2 có hai bản ghi với mức lương&nbsp;128922, 130000; trong đó 130000 là mức lương được cập nhật.
- emp_id 3 chỉ có một bản ghi lương, nên đó đã là mức lương được cập nhật.
- emp_id 4&nbsp;có hai bản ghi với mức lương&nbsp;162825, 170000; trong đó 170000 là mức lương được cập nhật.
- emp_id 5&nbsp;chỉ có một bản ghi lương, nên đó đã là mức lương được cập nhật.
- emp_id 6&nbsp;có hai bản ghi với mức lương 79632, 90000; trong đó 90000 là mức lương được cập nhật.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một nhân viên có thể có nhiều bản ghi lương; ta giữ lại mức lương cao nhất và sắp xếp theo $emp\_id$. Dùng `GROUP BY emp_id` với `MAX(salary)` là được vì các trường còn lại là duy nhất đối với mỗi nhân viên.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    emp_id,
    firstname,
    lastname,
    MAX(salary) AS salary,
    department_id
FROM Salary
GROUP BY emp_id
ORDER BY emp_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
