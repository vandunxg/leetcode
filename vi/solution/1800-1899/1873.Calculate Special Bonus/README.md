---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1873. Calculate Special Bonus](https://leetcode.com/problems/calculate-special-bonus)

[中文文档](/solution/1800-1899/1873.Calculate%20Special%20Bonus/README.md)

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
employee_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng của bảng biểu thị mã nhân viên, tên nhân viên và mức lương.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tính tiền thưởng của mỗi nhân viên. Tiền thưởng của một nhân viên bằng <code>100%</code> lương nếu mã nhân viên là <strong>số lẻ</strong> và <strong>tên nhân viên không bắt đầu bằng ký tự </strong><code>&#39;M&#39;</code>. Trong các trường hợp khác, tiền thưởng là <code>0</code>.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>employee_id</code>.</p>

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
| 3           | Michael | 3800   |
| 7           | Addilyn | 7400   |
| 8           | Juan    | 6100   |
| 9           | Kannon  | 7700   |
+-------------+---------+--------+
<strong>Đầu ra:</strong>
+-------------+-------+
| employee_id | bonus |
+-------------+-------+
| 2           | 0     |
| 3           | 0     |
| 7           | 7400  |
| 8           | 0     |
| 9           | 7700  |
+-------------+-------+
<strong>Giải thích:</strong>
Các nhân viên có mã 2 và 8 nhận tiền thưởng bằng 0 vì employee_id của họ là số chẵn.
Nhân viên có mã 3 nhận tiền thưởng bằng 0 vì tên bắt đầu bằng &#39;M&#39;.
Các nhân viên còn lại nhận tiền thưởng bằng 100% lương.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu lệnh IF + Mệnh đề ORDER BY

<!-- thinking:start -->

> **Tư duy**
>
> Nhân viên có mã lẻ và tên không bắt đầu bằng $M$ nhận tiền thưởng bằng mức lương; các trường hợp khác nhận $0$. Quy tắc này được đặt trong danh sách select.
>
> $\textit{IF}$ ánh xạ mã chẵn hoặc tên bắt đầu bằng $M$ thành $0$, các trường hợp khác thành $salary$, sau đó $\textit{ORDER BY}$ $employee\_id$.

<!-- thinking:end -->

Ta có thể dùng câu lệnh `IF` để xác định cách tính tiền thưởng, sau đó dùng `ORDER BY` để sắp xếp kết quả theo `employee_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    employee_id,
    IF(
        employee_id % 2 = 0
        OR LEFT(name, 1) = 'M',
        0,
        salary
    ) AS bonus
FROM
    employees
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
