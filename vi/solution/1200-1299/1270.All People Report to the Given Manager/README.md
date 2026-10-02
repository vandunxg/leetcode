---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1270. All People Report to the Given Manager 🔒](https://leetcode.com/problems/all-people-report-to-the-given-manager)

[中文文档](/solution/1200-1299/1270.All%20People%20Report%20to%20the%20Given%20Manager/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employees</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| employee_id   | int     |
| employee_name | varchar |
| manager_id    | int     |
+---------------+---------+
`employee_id` là cột có giá trị duy nhất trong bảng này.
Mỗi hàng cho biết nhân viên có ID `employee_id` và tên `employee_name` báo cáo công việc cho quản lý trực tiếp có ID `manager_id`.
Người đứng đầu công ty là nhân viên có `employee_id = 1`.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải tìm <code>employee_id</code> của tất cả nhân viên báo cáo trực tiếp hoặc gián tiếp cho người đứng đầu công ty.</p>

<p>Do công ty nhỏ, quan hệ báo cáo gián tiếp giữa các quản lý <strong>không vượt quá ba cấp quản lý</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Employees:
+-------------+---------------+------------+
| employee_id | employee_name | manager_id |
+-------------+---------------+------------+
| 1           | Boss          | 1          |
| 3           | Alice         | 3          |
| 2           | Bob           | 1          |
| 4           | Daniel        | 2          |
| 7           | Luis          | 4          |
| 8           | Jhon          | 3          |
| 9           | Angela        | 8          |
| 77          | Robert        | 1          |
+-------------+---------------+------------+
<strong>Đầu ra:</strong> 
+-------------+
| employee_id |
+-------------+
| 2           |
| 77          |
| 4           |
| 7           |
+-------------+
<strong>Giải thích:</strong> 
Người đứng đầu công ty là nhân viên có employee_id bằng 1.
Các nhân viên có employee_id là 2 và 77 báo cáo trực tiếp cho người đứng đầu công ty.
Nhân viên có employee_id là 4 báo cáo gián tiếp cho người đứng đầu công ty theo chuỗi 4 --&gt; 2 --&gt; 1. 
Nhân viên có employee_id là 7 báo cáo gián tiếp cho người đứng đầu công ty theo chuỗi 7 --&gt; 4 --&gt; 2 --&gt; 1.
Các nhân viên có employee_id là 3, 8 và 9 không báo cáo trực tiếp hoặc gián tiếp cho người đứng đầu công ty.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai phép JOIN

<!-- thinking:start -->

> **Tư duy**
>
> Cây phân cấp có tối đa ba cấp và CEO có ID $1$. Hai self-join lần theo $manager\_id$ đến quản lý cấp trên của quản lý; nếu ID đó là $1$ và nhân viên không phải người có ID $1$, nhân viên này báo cáo cho CEO. Số phép join tương ứng với giới hạn độ sâu của cây.

<!-- thinking:end -->

Ta có thể dùng hai phép join để tìm tất cả nhân viên báo cáo trực tiếp hoặc gián tiếp cho CEO.

Cụ thể, trước tiên ta join để tìm `manager_id` của quản lý cấp trên ứng với mỗi `manager_id`, rồi dùng một phép join khác để tìm `manager_id` của quản lý cấp cao hơn nữa. Cuối cùng, nếu `manager_id` của quản lý cấp cao hơn bằng $1$ và `employee_id` của nhân viên khác $1$, thì nhân viên đó báo cáo trực tiếp hoặc gián tiếp cho CEO.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT e1.employee_id
FROM
    Employees AS e1
    JOIN Employees AS e2 ON e1.manager_id = e2.employee_id
    JOIN Employees AS e3 ON e2.manager_id = e3.employee_id
WHERE e1.employee_id != 1 AND e3.manager_id = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
