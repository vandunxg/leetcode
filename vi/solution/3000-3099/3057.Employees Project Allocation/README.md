---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3057. Employees Project Allocation 🔒](https://leetcode.com/problems/employees-project-allocation)

[中文文档](/solution/3000-3099/3057.Employees%20Project%20Allocation/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Project</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
| workload    | int     |
+-------------+---------+
employee_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
employee_id là khóa ngoại (cột tham chiếu) đến bảng <code>Employee</code>.
Mỗi dòng trong bảng này cho biết nhân viên có employee_id đang làm việc trong dự án có project_id và workload của dự án đó.
</pre>

<p>Bảng: <code>Employees</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| team             | varchar |
+------------------+---------+
employee_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi dòng trong bảng này chứa thông tin về một nhân viên.
</pre>

<p>Viết lời giải để tìm các <strong>nhân viên</strong> được phân công vào những dự án có <strong>workload lớn hơn workload trung bình</strong> của tất cả nhân viên trong <strong>team tương ứng</strong>.</p>

<p>Trả về bảng kết quả được <em>sắp xếp theo</em> <code>employee_id</code>, <code>project_id</code> theo thứ tự <em><strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Project table:
+-------------+-------------+----------+
| project_id  | employee_id | workload |
+-------------+-------------+----------+
| 1           | 1           |  45      |
| 1           | 2           |  90      |
| 2           | 3           |  12      |
| 2           | 4           |  68      |
+-------------+-------------+----------+
Employees table:
+-------------+--------+------+
| employee_id | name   | team |
+-------------+--------+------+
| 1           | Khaled | A    |
| 2           | Ali    | B    |
| 3           | John   | B    |
| 4           | Doe    | A    |
+-------------+--------+------+
<strong>Đầu ra:</strong>
+-------------+------------+---------------+------------------+
| employee_id | project_id | employee_name | project_workload |
+-------------+------------+---------------+------------------+
| 2           | 1          | Ali           | 90               |
| 4           | 2          | Doe           | 68               |
+-------------+------------+---------------+------------------+
<strong>Giải thích:</strong>
- Nhân viên có ID 1 có workload dự án là 45 và thuộc Team A, nơi workload trung bình là 56.50. Vì workload dự án không lớn hơn workload trung bình của team nên nhân viên này bị loại.
- Nhân viên có ID 2 có workload dự án là 90 và thuộc Team B, nơi workload trung bình là 51.00. Vì workload dự án lớn hơn workload trung bình của team nên nhân viên này được đưa vào kết quả.
- Nhân viên có ID 3 có workload dự án là 12 và thuộc Team B, nơi workload trung bình là 51.00. Vì workload dự án không lớn hơn workload trung bình của team nên nhân viên này bị loại.
- Nhân viên có ID 4 có workload dự án là 68 và thuộc Team A, nơi workload trung bình là 56.50. Vì workload dự án lớn hơn workload trung bình của team nên nhân viên này được đưa vào kết quả.
Bảng kết quả được sắp xếp theo employee_id, project_id theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thống kê theo nhóm + Equi-Join

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần các dòng employee-project có workload lớn hơn workload trung bình của team. Giá trị trung bình này không chỉ có trong bảng nhân viên mà còn sử dụng workload của các dự án.
>
> Join các dự án với nhân viên, tính workload trung bình theo $\textit{team}$, rồi giữ lại các dòng có workload lớn hơn giá trị trung bình đó.
>
> Thực hiện hai lần merge (tính giá trị trung bình, rồi join ngược giá trị đó vào dữ liệu), sau đó sắp xếp theo employee và project id.

<!-- thinking:end -->

Trước tiên, chúng ta join bảng `Project` với bảng `Employees` dựa trên `employee_id`, sau đó group theo `team` để tính workload trung bình của mỗi team và lưu vào bảng tạm `T`.

Tiếp theo, chúng ta lại join bảng `Project` với bảng `Employees`, đồng thời join với bảng `T`, để tìm các nhân viên có workload lớn hơn workload trung bình của team. Cuối cùng, chúng ta sắp xếp theo `employee_id` và `project_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT team, AVG(workload) AS avg_workload
        FROM
            Project
            JOIN Employees USING (employee_id)
        GROUP BY 1
    )
SELECT
    employee_id,
    project_id,
    name AS employee_name,
    workload AS project_workload
FROM
    Project
    JOIN Employees USING (employee_id)
    JOIN T USING (team)
WHERE workload > avg_workload
ORDER BY 1, 2;
```

#### Python3

```python
import pandas as pd


def employees_with_above_avg_workload(
    project: pd.DataFrame, employees: pd.DataFrame
) -> pd.DataFrame:
    merged_df = pd.merge(project, employees, on="employee_id")
    avg_workload_per_team = merged_df.groupby("team")["workload"].mean().reset_index()
    merged_df = pd.merge(
        merged_df, avg_workload_per_team, on="team", suffixes=("", "_avg")
    )
    ans = merged_df[merged_df["workload"] > merged_df["workload_avg"]]
    ans = ans[["employee_id", "project_id", "name", "workload"]]
    ans = ans.rename(columns={"name": "employee_name", "workload": "project_workload"})
    return ans.sort_values(by=["employee_id", "project_id"])
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
