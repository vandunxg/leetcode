---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports)

[中文文档](/solution/0500-0599/0570.Managers%20with%20at%20Least%205%20Direct%20Reports/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employee</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này cho biết tên nhân viên, phòng ban và id của người quản lý họ.
Nếu managerId là null, nhân viên đó không có người quản lý.
Không có nhân viên nào tự quản lý chính mình.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm những người quản lý có ít nhất <strong>năm nhân viên báo cáo trực tiếp</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Employee:
+-----+-------+------------+-----------+
| id  | name  | department | managerId |
+-----+-------+------------+-----------+
| 101 | John  | A          | null      |
| 102 | Dan   | A          | 101       |
| 103 | James | A          | 101       |
| 104 | Amy   | A          | 101       |
| 105 | Anne  | A          | 101       |
| 106 | Ron   | B          | 101       |
+-----+-------+------------+-----------+
<strong>Đầu ra:</strong> 
+------+
| name |
+------+
| John |
+------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group và join

<!-- thinking:start -->

> **Tư duy**
>
> Người quản lý là nhân viên có ít nhất năm báo cáo trực tiếp. Đếm theo `managerId`, sau đó join lại để lấy tên.
>
> Gom nhóm và đếm số nhân viên báo cáo, giữ lại nhóm có $\ge 5$, rồi join với `Employee` theo id. Việc tổng hợp và tra cứu tên được thực hiện riêng.

<!-- thinking:end -->

Trước tiên, ta đếm số nhân viên báo cáo trực tiếp cho từng người quản lý, sau đó join với bảng `Employee` để tìm những người có ít nhất $5$ nhân viên báo cáo trực tiếp.

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def find_managers(employee: pd.DataFrame) -> pd.DataFrame:
    # Group the employees by managerId and count the number of direct reports
    manager_report_count = (
        employee.groupby("managerId").size().reset_index(name="directReports")
    )

    # Filter managers with at least five direct reports
    result = manager_report_count[manager_report_count["directReports"] >= 5]

    # Merge with the Employee table to get the names of these managers
    result = result.merge(
        employee[["id", "name"]], left_on="managerId", right_on="id", how="inner"
    )

    # Select only the 'name' column and drop the 'id' and 'directReports' columns
    result = result[["name"]]

    return result
```

#### MySQL

```sql
# Write your MySQL query statement below
SELECT name
FROM
    Employee
    JOIN (
        SELECT managerId AS id, COUNT(1) AS cnt
        FROM Employee
        GROUP BY 1
        HAVING cnt >= 5
    ) AS t
        USING (id);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
