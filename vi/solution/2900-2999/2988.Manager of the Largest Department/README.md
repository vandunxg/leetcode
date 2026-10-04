---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2988. Manager of the Largest Department 🔒](https://leetcode.com/problems/manager-of-the-largest-department)

[中文文档](/solution/2900-2999/2988.Manager%20of%20the%20Largest%20Department/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employees</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| emp_id      | int     |
| emp_name    | varchar |
| dep_id      | int     |
| position    | varchar |
+-------------+---------+
emp_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa emp_id, emp_name, dep_id và position.
</pre>

<p>Hãy viết lời giải để tìm <strong>tên</strong> của <strong>quản lý</strong> thuộc <strong>phòng ban lớn nhất</strong>. Có thể có nhiều phòng ban lớn nhất khi số lượng nhân viên trong các phòng ban đó bằng nhau.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>dep_id</code><em> theo thứ tự <strong>tăng dần</strong></em><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Employees table:
+--------+----------+--------+---------------+
| emp_id | emp_name | dep_id | position      |
+--------+----------+--------+---------------+
| 156    | Michael  | 107    | Manager       |
| 112    | Lucas    | 107    | Consultant    |
| 8      | Isabella | 101    | Manager       |
| 160    | Joseph   | 100    | Manager       |
| 80     | Aiden    | 100    | Engineer      |
| 190    | Skylar   | 100    | Freelancer    |
| 196    | Stella   | 101    | Coordinator   |
| 167    | Audrey   | 100    | Consultant    |
| 97     | Nathan   | 101    | Supervisor    |
| 128    | Ian      | 101    | Administrator |
| 81     | Ethan    | 107    | Administrator |
+--------+----------+--------+---------------+
<strong>Đầu ra</strong>
+--------------+--------+
| manager_name | dep_id |
+--------------+--------+
| Joseph       | 100    |
| Isabella     | 101    |
+--------------+--------+
<strong>Giải thích</strong>
- Các phòng ban có ID 100 và 101 đều có tổng cộng 4 nhân viên, trong khi phòng ban 107 có 3 nhân viên. Vì cả hai phòng ban 100 và 101 có cùng số lượng nhân viên, quản lý tương ứng của chúng đều được đưa vào kết quả.
Bảng kết quả được sắp xếp theo dep_id theo thứ tự tăng dần.

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm + Phép nối bằng + Truy vấn con

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số nhân viên trong từng phòng ban, nối với các hàng của quản lý, rồi giữ lại những phòng ban có số lượng bằng giá trị lớn nhất trên toàn bộ các phòng ban. Truy vấn con $MAX(cnt)$ giúp tránh phải nhóm lại các quản lý sau phép nối.
>
> Sắp xếp theo mã phòng ban.

<!-- thinking:end -->

Trước tiên, ta có thể đếm số nhân viên trong từng phòng ban và ký hiệu kết quả là bảng `T`. Sau đó, ta nối `T` với bảng `Employees`, với điều kiện nối là `T.dep_id = Employees.dep_id` và `Employees.position = 'Manager'`. Nhờ vậy, ta lấy được quản lý của từng phòng ban. Cuối cùng, ta lọc ra phòng ban có nhiều nhân viên nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT dep_id, COUNT(1) AS cnt
        FROM Employees
        GROUP BY 1
    )
SELECT emp_name AS manager_name, t.dep_id
FROM
    T AS t
    JOIN Employees AS e ON t.dep_id = e.dep_id AND e.position = 'Manager'
WHERE cnt = (SELECT MAX(cnt) FROM T)
ORDER BY 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
