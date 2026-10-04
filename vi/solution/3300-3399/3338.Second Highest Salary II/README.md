---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3338. Second Highest Salary II 🔒](https://leetcode.com/problems/second-highest-salary-ii)

[中文文档](/solution/3300-3399/3338.Second%20Highest%20Salary%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>employees</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| emp_id           | int     |
| salary           | int     |
| dept             | varchar |
+------------------+---------+
emp_id is the unique key for this table.
Each row of this table contains information about an employee including their ID, salary, and department.
</pre>

<p>Hãy viết lời giải để tìm những nhân viên nhận <strong>mức lương cao thứ hai</strong> trong mỗi phòng ban. Nếu <strong>nhiều nhân viên có mức lương cao thứ hai</strong>, hãy <strong>bao gồm</strong> <strong>tất cả nhân viên</strong> có <strong>mức lương đó</strong>.</p>

<p>Trả về <em>bảng kết quả</em> <em>được sắp xếp theo</em> <code>emp_id</code> <em>theo</em> <em><strong>thứ tự tăng dần</strong></em><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng employees:</p>

<pre class="example-io">
+--------+--------+-----------+
| emp_id | salary | dept      |
+--------+--------+-----------+
| 1      | 70000  | Sales     |
| 2      | 80000  | Sales     |
| 3      | 80000  | Sales     |
| 4      | 90000  | Sales     |
| 5      | 55000  | IT        |
| 6      | 65000  | IT        |
| 7      | 65000  | IT        |
| 8      | 50000  | Marketing |
| 9      | 55000  | Marketing |
| 10     | 55000  | HR        |
+--------+--------+-----------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+--------+-----------+
| emp_id | dept      |
+--------+-----------+
| 2      | Sales     |
| 3      | Sales     |
| 5      | IT        |
| 8      | Marketing |
+--------+-----------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Phòng Sales</strong>:

    <ul>
         <li>Mức lương cao nhất là 90000 (emp_id: 4)</li>
         <li>Mức lương cao thứ hai là 80000 (emp_id: 2, 3)</li>
         <li>Cả hai nhân viên có mức lương 80000 đều được đưa vào kết quả</li>
    </ul>
    </li>
    <li><strong>Phòng IT</strong>:
    <ul>
         <li>Mức lương cao nhất là 65000 (emp_id: 6, 7)</li>
         <li>Mức lương cao thứ hai là 55000 (emp_id: 5)</li>
         <li>Chỉ nhân viên có emp_id 5 được đưa vào kết quả vì họ có mức lương cao thứ hai</li>
    </ul>
    </li>
    <li><strong>Phòng Marketing</strong>:
    <ul>
         <li>Mức lương cao nhất là 55000 (emp_id: 9)</li>
         <li>Mức lương cao thứ hai là 50000 (emp_id: 8)</li>
         <li>Nhân viên 8&nbsp;được đưa vào kết quả</li>
    </ul>
    </li>
    <li><strong>Phòng HR</strong>:
    <ul>
         <li>Chỉ có một nhân viên</li>
         <li>Không được đưa vào kết quả vì phòng ban này có ít hơn 2 nhân viên</li>
    </ul>
    </li>

</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phòng ban cần trả về mọi nhân viên có mức lương cao thứ hai. Các mức lương bằng nhau phải có cùng thứ hạng, vì vậy ta dùng dense rank thay vì row number.
>
> Xếp hạng $\textit{salary}$ theo thứ tự giảm dần trong từng $\textit{dept}$, giữ lại các dòng có $\textit{rk}=2$, rồi sắp xếp theo $\textit{emp\_id}$.
>
> Khi đó, mọi nhân viên đồng hạng hai trong một phòng ban đều xuất hiện trong kết quả.

<!-- thinking:end -->

Có thể sử dụng hàm cửa sổ `DENSE_RANK()` để xếp hạng nhân viên trong từng phòng ban theo mức lương giảm dần, sau đó lọc những nhân viên có thứ hạng $2$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            emp_id,
            dept,
            DENSE_RANK() OVER (
                PARTITION BY dept
                ORDER BY salary DESC
            ) rk
        FROM Employees
    )
SELECT emp_id, dept
FROM T
WHERE rk = 2
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def find_second_highest_salary(employees: pd.DataFrame) -> pd.DataFrame:
    employees["rk"] = employees.groupby("dept")["salary"].rank(
        method="dense", ascending=False
    )
    second_highest = employees[employees["rk"] == 2][["emp_id", "dept"]]
    return second_highest.sort_values(by="emp_id")
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
