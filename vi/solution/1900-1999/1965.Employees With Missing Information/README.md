---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1965. Employees With Missing Information](https://leetcode.com/problems/employees-with-missing-information)

[中文文档](/solution/1900-1999/1965.Employees%20With%20Missing%20Information/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employees</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| name        | varchar |
+-------------+---------+
employee_id là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng của bảng này cho biết tên của nhân viên có ID là employee_id.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Salaries</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| salary      | int     |
+-------------+---------+
employee_id là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng của bảng này cho biết mức lương của nhân viên có ID là employee_id.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo ID của tất cả nhân viên bị <strong>thiếu thông tin</strong>. Thông tin của một nhân viên bị thiếu nếu:</p>

<ul>
	<li><strong>tên</strong> của nhân viên bị thiếu, hoặc</li>
	<li><strong>mức lương</strong> của nhân viên bị thiếu.</li>
</ul>

<p>Trả về bảng kết quả được sắp xếp theo <code>employee_id</code> <strong>theo thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Employees:
+-------------+----------+
| employee_id | name     |
+-------------+----------+
| 2           | Crew     |
| 4           | Haven    |
| 5           | Kristian |
+-------------+----------+
Bảng Salaries:
+-------------+--------+
| employee_id | salary |
+-------------+--------+
| 5           | 76071  |
| 1           | 22517  |
| 4           | 63539  |
+-------------+--------+
<strong>Đầu ra:</strong>
+-------------+
| employee_id |
+-------------+
| 1           |
| 2           |
+-------------+
<strong>Giải thích:</strong>
Nhân viên 1, 2, 4 và 5 đang làm việc tại công ty này.
Tên của nhân viên 1 bị thiếu.
Mức lương của nhân viên 2 bị thiếu.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Subquery + Union

<!-- thinking:start -->

> **Tư duy**
>
> Thiếu thông tin nghĩa là một ID chỉ xuất hiện trong một trong hai bảng. Ta thực hiện cả hai phép anti-join rồi sắp xếp.
>
> Các subquery $\texttt{NOT IN}$ thu thập những ID chỉ có trong $\texttt{Employees}$ hoặc chỉ có trong $\texttt{Salaries}$; $\texttt{UNION}$ và $\texttt{ORDER BY}$ hoàn tất kết quả.

<!-- thinking:end -->

Trước tiên, ta có thể tìm tất cả `employee_id` không có trong bảng `Salaries` nhưng có trong bảng `Employees`, sau đó tìm tất cả `employee_id` không có trong bảng `Employees` nhưng có trong bảng `Salaries`. Cuối cùng, ta kết hợp hai kết quả bằng toán tử `UNION` và sắp xếp kết quả theo `employee_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT employee_id
FROM Employees
WHERE employee_id NOT IN (SELECT employee_id FROM Salaries)
UNION
SELECT employee_id
FROM Salaries
WHERE employee_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
