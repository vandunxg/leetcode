---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1875. Group Employees of the Same Salary 🔒](https://leetcode.com/problems/group-employees-of-the-same-salary)

[中文文档](/solution/1800-1899/1875.Group%20Employees%20of%20the%20Same%20Salary/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employees</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| name        | varchar |
| salary      | int     |
+-------------+---------+
employee_id là cột có các giá trị duy nhất của bảng này.
Mỗi hàng của bảng biểu thị mã nhân viên, tên nhân viên và mức lương.
</pre>

<p>&nbsp;</p>

<p>Một công ty muốn chia nhân viên thành các nhóm sao cho mọi thành viên trong mỗi nhóm có <strong>cùng mức lương</strong>. Các nhóm phải tuân theo những tiêu chí sau:</p>

<ul>
	<li>Mỗi nhóm phải có <strong>ít nhất hai</strong> nhân viên.</li>
	<li>Tất cả nhân viên trong một nhóm phải có <strong>cùng mức lương</strong>.</li>
	<li>Tất cả nhân viên có cùng mức lương phải được xếp vào cùng một nhóm.</li>
	<li>Nếu mức lương của một nhân viên là duy nhất, ta <strong>không</strong> xếp nhân viên đó vào nhóm nào.</li>
	<li>ID của nhóm được gán dựa trên <strong>thứ hạng mức lương của nhóm</strong> so với mức lương của các nhóm khác, trong đó nhóm có mức lương <strong>thấp nhất</strong> có <code>team_id = 1</code>. Lưu ý rằng mức lương của những nhân viên không thuộc nhóm nào <strong>không được tính</strong> vào thứ hạng này.</li>
</ul>

<p>Viết lời giải để lấy <code>team_id</code> của mỗi nhân viên thuộc một nhóm.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>team_id</code> <strong>tăng dần</strong>. Nếu bằng nhau, sắp xếp theo <code>employee_id</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Employees:
+-------------+---------+--------+
| employee_id | name    | salary |
+-------------+---------+--------+
| 2           | Meir    | 3000   |
| 3           | Michael | 3000   |
| 7           | Addilyn | 7400   |
| 8           | Juan    | 6100   |
| 9           | Kannon  | 7400   |
+-------------+---------+--------+
<strong>Đầu ra:</strong>
+-------------+---------+--------+---------+
| employee_id | name    | salary | team_id |
+-------------+---------+--------+---------+
| 2           | Meir    | 3000   | 1       |
| 3           | Michael | 3000   | 1       |
| 7           | Addilyn | 7400   | 2       |
| 9           | Kannon  | 7400   | 2       |
+-------------+---------+--------+---------+
<strong>Giải thích:</strong>
Meir (employee_id=2) và Michael (employee_id=3) thuộc cùng một nhóm vì họ có cùng mức lương 3000.
Addilyn (employee_id=7) và Kannon (employee_id=9) thuộc cùng một nhóm vì họ có cùng mức lương 7400.
Juan (employee_id=8) không thuộc nhóm nào vì mức lương 6100 của họ là duy nhất (tức là không có nhân viên nào khác có cùng mức lương).
ID nhóm được gán như sau (dựa trên thứ hạng mức lương, từ thấp nhất):
- team_id=1: Meir và Michael, mức lương 3000
- team_id=2: Addilyn và Kannon, mức lương 7400
Mức lương 6100 của Juan không được tính vào thứ hạng vì họ không thuộc nhóm nào.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Những nhân viên có cùng mức lương với ít nhất một người khác tạo thành một nhóm, được đánh số theo thứ hạng của mức lương đó; các mức lương duy nhất bị bỏ qua. Trước hết ta cần tìm các mức lương lặp lại, sau đó xếp hạng chúng.
>
> Gom nhóm theo $salary$ với $\textit{HAVING COUNT}>1$, gán $team\_id$ bằng $ROW\_NUMBER$ theo thứ tự mức lương, rồi join ngược lại với Employees.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT salary
        FROM Employees
        GROUP BY salary
        HAVING COUNT(1) > 1
    ),
    T AS (
        SELECT salary, ROW_NUMBER() OVER (ORDER BY salary) AS team_id
        FROM S
    )
SELECT e.*, t.team_id
FROM
    Employees AS e
    JOIN T AS t ON e.salary = t.salary
ORDER BY 4, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
