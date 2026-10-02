---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1303. Find the Team Size 🔒](https://leetcode.com/problems/find-the-team-size)

[中文文档](/solution/1300-1399/1303.Find%20the%20Team%20Size/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employee</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| employee_id   | int     |
| team_id       | int     |
+---------------+---------+
employee_id là primary key (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng chứa ID của một nhân viên và team tương ứng của họ.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm quy mô team của từng nhân viên.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Employee:
+-------------+------------+
| employee_id | team_id    |
+-------------+------------+
|     1       |     8      |
|     2       |     8      |
|     3       |     8      |
|     4       |     7      |
|     5       |     9      |
|     6       |     9      |
+-------------+------------+
<strong>Đầu ra:</strong> 
+-------------+------------+
| employee_id | team_size  |
+-------------+------------+
|     1       |     3      |
|     2       |     3      |
|     3       |     3      |
|     4       |     1      |
|     5       |     2      |
|     6       |     2      |
+-------------+------------+
<strong>Giải thích:</strong> 
Các nhân viên có ID 1, 2, 3 thuộc team có team_id = 8.
Nhân viên có ID 4 thuộc team có team_id = 7.
Các nhân viên có ID 5, 6 thuộc team có team_id = 9.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By + Equi-Join

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm quy mô team của từng nhân viên. Nếu đếm trực tiếp từ các hàng nhân viên thì sẽ phải tính lại nhiều lần cho cùng $\textit{team\_id}$. Group theo $\textit{team\_id}$ trước sẽ cho quy mô từng team; sau đó equi-join theo key này để gắn quy mô vào hàng của mỗi nhân viên.

<!-- thinking:end -->

Trước tiên, ta đếm số nhân viên trong từng team và lưu vào bảng `T`. Sau đó, dùng equi-join giữa bảng `Employee` và `T` theo `team_id` để lấy tổng số nhân viên của team tương ứng.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT team_id, COUNT(1) AS team_size
        FROM Employee
        GROUP BY 1
    )
SELECT employee_id, team_size
FROM
    Employee
    JOIN T USING (team_id);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Truy vấn trước cần một bảng trung gian. Ta có thể self-join $\textit{Employee}$ theo $\textit{team\_id}$ rồi group theo $\textit{employee\_id}$; phép join ghép các thành viên cùng team và tính quy mô team trong một câu lệnh, không cần tạo CTE đã group.

<!-- thinking:end -->

Ta cũng có thể left join bảng `Employee` với chính nó theo `team_id`, rồi group theo `employee_id` để đếm tổng số nhân viên trong team của nhân viên đó.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT e1.employee_id, COUNT(1) AS team_size
FROM
    Employee AS e1
    LEFT JOIN Employee AS e2 USING (team_id)
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
