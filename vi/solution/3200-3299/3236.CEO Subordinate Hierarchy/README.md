---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3236. CEO Subordinate Hierarchy 🔒](https://leetcode.com/problems/ceo-subordinate-hierarchy)

[中文文档](/solution/3200-3299/3236.CEO%20Subordinate%20Hierarchy/README.md)

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
| salary        | int     |
+---------------+---------+
employee_id là định danh duy nhất của bảng này.
manager_id là employee_id của người quản lý nhân viên. CEO có manager_id là NULL.
</pre>

<p>Hãy viết lời giải để tìm các cấp dưới của CEO (cả <strong>trực tiếp</strong> và <strong>gián tiếp</strong>), cùng với <strong>cấp bậc trong hệ thống phân cấp</strong> và <strong>chênh lệch lương</strong> so với CEO.</p>

<p>Kết quả cần có các cột sau:</p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<ul>
	<li><code>subordinate_id</code>: employee_id của cấp dưới</li>
	<li><code>subordinate_name</code>: tên của cấp dưới</li>
	<li><code>hierarchy_level</code>: cấp bậc của cấp dưới trong hệ thống phân cấp (<code>1</code> cho nhân viên <strong>báo cáo trực tiếp</strong>, <code>2</code> cho <strong>nhân viên báo cáo trực tiếp của họ</strong>, và <strong>tiếp tục như vậy</strong>)</li>
	<li><code>salary_difference</code>: chênh lệch giữa lương của cấp dưới và lương của CEO</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>hierarchy_level</code> <em><strong>tăng dần</strong></em>, <em>sau đó theo</em> <code>subordinate_id</code> <em><strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>Employees</code>:</p>

<pre class="example-io">
+-------------+----------------+------------+---------+
| employee_id | employee_name  | manager_id | salary  |
+-------------+----------------+------------+---------+
| 1           | Alice          | NULL       | 150000  |
| 2           | Bob            | 1          | 120000  |
| 3           | Charlie        | 1          | 110000  |
| 4           | David          | 2          | 105000  |
| 5           | Eve            | 2          | 100000  |
| 6           | Frank          | 3          | 95000   |
| 7           | Grace          | 3          | 98000   |
| 8           | Helen          | 5          | 90000   |
+-------------+----------------+------------+---------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+----------------+------------------+------------------+-------------------+
| subordinate_id | subordinate_name | hierarchy_level  | salary_difference |
+----------------+------------------+------------------+-------------------+
| 2              | Bob              | 1                | -30000            |
| 3              | Charlie          | 1                | -40000            |
| 4              | David            | 2                | -45000            |
| 5              | Eve              | 2                | -50000            |
| 6              | Frank            | 2                | -55000            |
| 7              | Grace            | 2                | -52000            |
| 8              | Helen            | 3                | -60000            |
+----------------+------------------+------------------+-------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bob và Charlie là cấp dưới trực tiếp của Alice (CEO), nên có hierarchy_level bằng 1.</li>
	<li>David và Eve báo cáo cho Bob, còn Frank và Grace báo cáo cho Charlie, nên là cấp dưới cấp 2 (hierarchy_level 2).</li>
	<li>Helen báo cáo cho Eve, nên là cấp dưới cấp 3 (hierarchy_level 3).</li>
	<li>Chênh lệch lương được tính dựa trên mức lương 150000 của Alice.</li>
	<li>Kết quả được sắp xếp theo hierarchy_level tăng dần, sau đó theo subordinate_id tăng dần.</li>
</ul>

<p><strong>Ghi chú:</strong> Kết quả được sắp xếp trước theo hierarchy_level tăng dần, sau đó theo subordinate_id tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Recursive CTE + Join

<!-- thinking:start -->

> **Tư duy**
>
> Sơ đồ tổ chức là một cây có gốc tại CEO; ta cần độ sâu và khoảng chênh lệch lương của từng cấp dưới. Việc self-join lặp lại không thể giới hạn độ sâu.
>
> CTE đệ quy bắt đầu từ `manager_id IS NULL` và duyệt theo các cạnh quản lý, tăng độ sâu; lấy lương CEO để trừ, loại bỏ cấp $0$, rồi sắp xếp theo độ sâu và id.

<!-- thinking:end -->

Trước tiên, sử dụng một CTE đệ quy để tính cấp bậc trong hệ thống phân cấp của mỗi nhân viên, trong đó CEO có cấp $0$. Lưu `employee_id`, `employee_name`, `hierarchy_level`, `manager_id` và `salary` vào bảng tạm `T`.

Tiếp theo, truy vấn lương của CEO và lưu vào bảng tạm `P`.

Cuối cùng, join các bảng `T` và `P` để tính chênh lệch lương cho mỗi cấp dưới, rồi sắp xếp theo `hierarchy_level` và `subordinate_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH RECURSIVE
    T AS (
        SELECT
            employee_id,
            employee_name,
            0 AS hierarchy_level,
            manager_id,
            salary
        FROM Employees
        WHERE manager_id IS NULL
        UNION ALL
        SELECT
            e.employee_id,
            e.employee_name,
            hierarchy_level + 1 AS hierarchy_level,
            e.manager_id,
            e.salary
        FROM
            T t
            JOIN Employees e ON t.employee_id = e.manager_id
    ),
    P AS (
        SELECT salary
        FROM Employees
        WHERE manager_id IS NULL
    )
SELECT
    employee_id subordinate_id,
    employee_name subordinate_name,
    hierarchy_level,
    t.salary - p.salary salary_difference
FROM
    T t
    JOIN P p
WHERE hierarchy_level != 0
ORDER BY 3, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
