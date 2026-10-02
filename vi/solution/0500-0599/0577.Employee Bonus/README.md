---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [577. Employee Bonus](https://leetcode.com/problems/employee-bonus)

[中文文档](/solution/0500-0599/0577.Employee%20Bonus/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employee</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
empId là cột có giá trị duy nhất trong bảng này.
Mỗi hàng của bảng chứa tên, mã nhân viên, mức lương và mã người quản lý của một nhân viên.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Bonus</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
empId là cột có giá trị duy nhất trong bảng này.
empId là khóa ngoại tham chiếu đến empId trong bảng Employee.
Mỗi hàng của bảng chứa mã nhân viên và tiền thưởng tương ứng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết truy vấn trả về tên và tiền thưởng của mỗi nhân viên thỏa mãn một trong các điều kiện sau:</p>

<ul>
	<li>Nhân viên có tiền thưởng <strong>nhỏ hơn</strong> <code>1000</code>.</li>
	<li>Nhân viên không nhận được tiền thưởng.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Employee:
+-------+--------+------------+--------+
| empId | name   | supervisor | salary |
+-------+--------+------------+--------+
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |
+-------+--------+------------+--------+
Bảng Bonus:
+-------+-------+
| empId | bonus |
+-------+-------+
| 2     | 500   |
| 4     | 2000  |
+-------+-------+
<strong>Đầu ra:</strong> 
+------+-------+
| name | bonus |
+------+-------+
| Brad | null  |
| John | null  |
| Dan  | 500   |
+------+-------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê nhân viên có tiền thưởng dưới $1000$, xem trường hợp không có tiền thưởng là $0$. Inner join sẽ loại những người không có hàng tương ứng trong bảng tiền thưởng.
>
> Left join với `Bonus` rồi lọc bằng `IFNULL(bonus, 0) < 1000`. `NULL` được chuyển thành $0$, tương ứng với trường hợp không nhận tiền thưởng.

<!-- thinking:end -->

Ta có thể dùng left join để nối bảng `Employee` với bảng `Bonus` theo `empId`, sau đó lọc những nhân viên có tiền thưởng dưới $1000$. Cũng cần giữ lại nhân viên có giá trị tiền thưởng là `NULL` sau phép join, nên dùng hàm `IFNULL` để chuyển `NULL` thành $0$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT name, bonus
FROM
    Employee
    LEFT JOIN Bonus USING (empId)
WHERE IFNULL(bonus, 0) < 1000;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
