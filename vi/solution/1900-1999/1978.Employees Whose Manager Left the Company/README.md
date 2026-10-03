---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1978. Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company)

[中文文档](/solution/1900-1999/1978.Employees%20Whose%20Manager%20Left%20the%20Company/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employees</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| manager_id  | int      |
| salary      | int      |
+-------------+----------+
Trong SQL, employee_id là khóa chính của bảng này.
Bảng này chứa thông tin về nhân viên, mức lương và ID của quản lý. Một số nhân viên không có quản lý (manager_id là null).
</pre>

<p>&nbsp;</p>

<p>Tìm ID của những nhân viên có mức lương nhỏ hơn <code>$30000</code> và có quản lý đã rời công ty. Khi một quản lý rời công ty, thông tin của người đó bị xóa khỏi bảng <code>Employees</code>, nhưng các nhân viên cấp dưới vẫn giữ <code>manager_id</code> của quản lý đã rời đi.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>employee_id</code>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Employees:
+-------------+-----------+------------+--------+
| employee_id | name      | manager_id | salary |
+-------------+-----------+------------+--------+
| 3           | Mila      | 9          | 60301  |
| 12          | Antonella | null       | 31000  |
| 13          | Emery     | null       | 67084  |
| 1           | Kalel     | 11         | 21241  |
| 9           | Mikaela   | null       | 50937  |
| 11          | Joziah    | 6          | 28485  |
+-------------+-----------+------------+--------+
<strong>Đầu ra:</strong>
+-------------+
| employee_id |
+-------------+
| 11          |
+-------------+

<strong>Giải thích:</strong>
Những nhân viên có mức lương nhỏ hơn $30000 là 1 (Kalel) và 11 (Joziah).
Quản lý của Kalel là nhân viên 11, người vẫn còn làm việc tại công ty (Joziah).
Quản lý của Joziah là nhân viên 6, người đã rời công ty vì không còn hàng nào cho nhân viên 6 do hàng đó đã bị xóa.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm những nhân viên có lương dưới $30000$ và có manager id nhưng người quản lý không còn trong bảng. Một self left-join không khớp với hàng của người quản lý sẽ phát hiện quản lý đã rời công ty.

<!-- thinking:end -->

Ta có thể dùng left join để nối bảng nhân viên với chính nó, sau đó lọc những nhân viên có mức lương nhỏ hơn $30000$ và có quản lý trực tiếp đã rời công ty.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT e1.employee_id
FROM
    Employees AS e1
    LEFT JOIN Employees AS e2 ON e1.manager_id = e2.employee_id
WHERE e1.salary < 30000 AND e1.manager_id IS NOT NULL AND e2.employee_id IS NULL
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Subquery

<!-- thinking:start -->

> **Tư duy**
>
> Phép join có thể được thay bằng $\texttt{NOT IN}$: manager id không xuất hiện trong $\texttt{employee\_id}$. Ý nghĩa tương đương và truy vấn ngắn hơn.

<!-- thinking:end -->

Ta cũng có thể dùng subquery để tìm trước tất cả quản lý đã rời công ty, sau đó tìm những nhân viên có mức lương nhỏ hơn $30000$ và quản lý trực tiếp không nằm trong danh sách các quản lý đã rời công ty.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT employee_id
FROM Employees
WHERE salary < 30000 AND manager_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
