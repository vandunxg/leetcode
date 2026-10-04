---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3482. Analyze Organization Hierarchy](https://leetcode.com/problems/analyze-organization-hierarchy)

[中文文档](/solution/3400-3499/3482.Analyze%20Organization%20Hierarchy/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employees</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| employee_id    | int     |
| employee_name  | varchar |
| manager_id     | int     |
| salary         | int     |
| department     | varchar |
+----------------+----------+
employee_id là khóa duy nhất của bảng này.
Mỗi hàng chứa thông tin về một nhân viên, bao gồm ID, tên, ID của manager, lương và phòng ban của họ.
manager_id là null đối với manager cấp cao nhất (CEO).
</pre>

<p>Hãy viết lời giải để phân tích hệ thống phân cấp của tổ chức và trả lời các câu hỏi sau:</p>

<ol>
    <li><strong>Cấp bậc:</strong> Với mỗi nhân viên, xác định cấp bậc của họ trong tổ chức (CEO ở cấp <code>1</code>, nhân viên báo cáo trực tiếp cho CEO ở cấp <code>2</code>, v.v.).</li>
    <li><strong>Quy mô đội nhóm:</strong> Với mỗi nhân viên là manager, đếm tổng số nhân viên dưới quyền họ (bao gồm cả nhân viên báo cáo trực tiếp và gián tiếp).</li>
    <li><strong>Ngân sách lương:</strong> Với mỗi manager, tính tổng ngân sách lương mà họ quản lý (tổng lương của tất cả nhân viên dưới quyền, bao gồm cả nhân viên báo cáo gián tiếp, cộng với lương của chính họ).</li>
</ol>

<p>Trả về <em>bảng kết quả được&nbsp;<em>sắp xếp theo <strong>level</strong> <strong>tăng dần</strong>, sau đó theo <strong>budget</strong> <strong>giảm dần</strong> và cuối cùng theo <strong>employee_name</strong> <strong>tăng dần</strong></em>.</em></p>

<p><em>Định dạng kết quả như trong ví dụ sau.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Employees:</p>

<pre class="example-io">
+-------------+---------------+------------+--------+-------------+
| employee_id | employee_name | manager_id | salary | department  |
+-------------+---------------+------------+--------+-------------+
| 1           | Alice         | null       | 12000  | Executive   |
| 2           | Bob           | 1          | 10000  | Sales       |
| 3           | Charlie       | 1          | 10000  | Engineering |
| 4           | David         | 2          | 7500   | Sales       |
| 5           | Eva           | 2          | 7500   | Sales       |
| 6           | Frank         | 3          | 9000   | Engineering |
| 7           | Grace         | 3          | 8500   | Engineering |
| 8           | Hank          | 4          | 6000   | Sales       |
| 9           | Ivy           | 6          | 7000   | Engineering |
| 10          | Judy          | 6          | 7000   | Engineering |
+-------------+---------------+------------+--------+-------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+---------------+-------+-----------+--------+
| employee_id | employee_name | level | team_size | budget |
+-------------+---------------+-------+-----------+--------+
| 1           | Alice         | 1     | 9         | 84500  |
| 3           | Charlie       | 2     | 4         | 41500  |
| 2           | Bob           | 2     | 3         | 31000  |
| 6           | Frank         | 3     | 2         | 23000  |
| 4           | David         | 3     | 1         | 13500  |
| 7           | Grace         | 3     | 0         | 8500   |
| 5           | Eva           | 3     | 0         | 7500   |
| 9           | Ivy           | 4     | 0         | 7000   |
| 10          | Judy          | 4     | 0         | 7000   |
| 8           | Hank          | 4     | 0         | 6000   |
+-------------+---------------+-------+-----------+--------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Cấu trúc tổ chức:</strong>

    <ul>
        <li>Alice (ID: 1) là CEO (cấp 1) và không có manager</li>
        <li>Bob (ID: 2) và Charlie (ID: 3) báo cáo trực tiếp cho Alice (cấp 2)</li>
        <li>David (ID: 4), Eva (ID: 5) báo cáo cho Bob, còn Frank (ID: 6) và Grace (ID: 7) báo cáo cho Charlie (cấp 3)</li>
        <li>Hank (ID: 8) báo cáo cho David, còn Ivy (ID: 9) và Judy (ID: 10) báo cáo cho Frank (cấp 4)</li>
    </ul>
    </li>
    <li><strong>Tính cấp bậc:</strong>
    <ul>
        <li>CEO (Alice) ở cấp 1</li>
        <li>Mỗi cấp quản lý tiếp theo tăng cấp bậc thêm 1</li>
    </ul>
    </li>
    <li><strong>Tính quy mô đội nhóm:</strong>
    <ul>
        <li>Alice có 9 nhân viên dưới quyền (toàn bộ công ty ngoại trừ cô ấy)</li>
        <li>Bob có 3 nhân viên (David, Eva và Hank)</li>
        <li>Charlie có 4 nhân viên (Frank, Grace, Ivy và Judy)</li>
        <li>David có 1 nhân viên (Hank)</li>
        <li>Frank có 2 nhân viên (Ivy và Judy)</li>
        <li>Eva, Grace, Hank, Ivy và Judy không có nhân viên báo cáo trực tiếp nào (team_size = 0)</li>
    </ul>
    </li>
    <li><strong>Tính ngân sách:</strong>
    <ul>
        <li>Ngân sách của Alice: Lương của cô ấy (12000) + lương của tất cả nhân viên (72500) = 84500</li>
        <li>Ngân sách của Charlie: Lương của anh ấy (10000) + ngân sách của Frank (23000) + lương của Grace (8500) = 41500</li>
        <li>Ngân sách của Bob: Lương của anh ấy (10000) + ngân sách của David (13500) + lương của Eva (7500) = 31000</li>
        <li>Ngân sách của Frank: Lương của anh ấy (9000) + lương của Ivy (7000) + lương của Judy (7000) = 23000</li>
        <li>Ngân sách của David: Lương của anh ấy (7500) + lương của Hank (6000) = 13500</li>
        <li>Nhân viên không có nhân viên báo cáo trực tiếp có ngân sách bằng lương của chính họ</li>
    </ul>
    </li>

</ul>

<p><strong>Lưu ý:</strong></p>

<ul>
    <li>Kết quả trước tiên được sắp xếp theo cấp bậc tăng dần</li>
    <li>Trong cùng một cấp, nhân viên được sắp xếp theo ngân sách giảm dần, sau đó theo tên tăng dần</li>
</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các con trỏ manager tạo thành một cây; ta cần cấp bậc, quy mô đội nhóm và tổng lương của đội nhóm cho mỗi người. CEO có $\textit{manager\_id}$ là null.
>
> Các cấp bậc được mở rộng từ trên xuống dưới, bắt đầu từ tập manager hiện tại. Quy mô đội nhóm và ngân sách phải chờ xử lý xong các node con, nên ta cộng dồn theo thứ tự id giảm dần (hoặc duyệt hậu tự).
>
> Đánh dấu CEO ở cấp $1$, đệ quy để gắn cấp cho các nhân viên báo cáo trực tiếp bằng $level+1$, cộng dồn $\textit{team\_size}$ và $\textit{budget}$, rồi sắp xếp theo cấp bậc, ngân sách giảm dần và tên.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH RECURSIVE
    level_cte AS (
        SELECT employee_id, manager_id, 1 AS level, salary FROM Employees
        UNION ALL
        SELECT a.employee_id, b.manager_id, level + 1, a.salary
        FROM
            level_cte a
            JOIN Employees b ON b.employee_id = a.manager_id
    ),
    employee_with_level AS (
        SELECT a.employee_id, a.employee_name, a.salary, b.level
        FROM
            Employees a,
            (SELECT employee_id, level FROM level_cte WHERE manager_id IS NULL) b
        WHERE a.employee_id = b.employee_id
    )
SELECT
    a.employee_id,
    a.employee_name,
    a.level,
    COALESCE(b.team_size, 0) AS team_size,
    a.salary + COALESCE(b.budget, 0) AS budget
FROM
    employee_with_level a
    LEFT JOIN (
        SELECT manager_id AS employee_id, COUNT(*) AS team_size, SUM(salary) AS budget
        FROM level_cte
        WHERE manager_id IS NOT NULL
        GROUP BY manager_id
    ) b
        ON a.employee_id = b.employee_id
ORDER BY level, budget DESC, employee_name;
```

#### Pandas

```python
import pandas as pd

def analyze_organization_hierarchy(employees: pd.DataFrame) -> pd.DataFrame:
    # Copy the input DataFrame to avoid modifying the original
    employees = employees.copy()
    employees['level'] = None

    # Identify the CEO (level 1)
    ceo_id = employees.loc[employees['manager_id'].isna(), 'employee_id'].values[0]
    employees.loc[employees['employee_id'] == ceo_id, 'level'] = 1

    # Recursively compute employee levels
    def compute_levels(emp_df, level):
        next_level_ids = emp_df[emp_df['level'] == level]['employee_id'].tolist()
        if not next_level_ids:
            return
        emp_df.loc[emp_df['manager_id'].isin(next_level_ids), 'level'] = level + 1
        compute_levels(emp_df, level + 1)

    compute_levels(employees, 1)

    # Initialize team size and budget dictionaries
    team_size = {eid: 0 for eid in employees['employee_id']}
    budget = {eid: salary for eid, salary in zip(employees['employee_id'], employees['salary'])}

    # Compute team size and budget for each employee
    for eid in sorted(employees['employee_id'], reverse=True):
        manager_id = employees.loc[employees['employee_id'] == eid, 'manager_id'].values[0]
        if pd.notna(manager_id):
            team_size[manager_id] += team_size[eid] + 1
            budget[manager_id] += budget[eid]

    # Map computed team size and budget to employees DataFrame
    employees['team_size'] = employees['employee_id'].map(team_size)
    employees['budget'] = employees['employee_id'].map(budget)

    # Sort the final result by level (ascending), budget (descending), and employee name (ascending)
    employees = employees.sort_values(by=['level', 'budget', 'employee_name'], ascending=[True, False, True])

    return employees[['employee_id', 'employee_name', 'level', 'team_size', 'budget']]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
