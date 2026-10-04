---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2879. Display the First Three Rows](https://leetcode.com/problems/display-the-first-three-rows)

[中文文档](/solution/2800-2899/2879.Display%20the%20First%20Three%20Rows/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame: <code>employees</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| employee_id | int    |
| name        | object |
| department  | object |
| salary      | int    |
+-------------+--------+
</pre>

<p>Hãy viết lời giải để hiển thị <strong><code>3</code> </strong>hàng<strong> </strong>đầu tiên của DataFrame này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>DataFrame employees
+-------------+-----------+-----------------------+--------+
| employee_id | name      | department            | salary |
+-------------+-----------+-----------------------+--------+
| 3           | Bob       | Operations            | 48675  |
| 90          | Alice     | Sales                 | 11096  |
| 9           | Tatiana   | Engineering           | 33805  |
| 60          | Annabelle | InformationTechnology | 37678  |
| 49          | Jonathan  | HumanResources        | 23793  |
| 43          | Khaled    | Administration        | 40454  |
+-------------+-----------+-----------------------+--------+
<strong>Đầu ra:</strong>
+-------------+---------+-------------+--------+
| employee_id | name    | department  | salary |
+-------------+---------+-------------+--------+
| 3           | Bob     | Operations  | 48675  |
| 90          | Alice   | Sales       | 11096  |
| 9           | Tatiana | Engineering | 33805  |
+-------------+---------+-------------+--------+
<strong>Giải thích:</strong>
Chỉ hiển thị 3 hàng đầu tiên.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ cần lấy ba hàng đầu tiên. `head(3)` lấy các hàng này theo thứ tự được lưu mà không cần thêm bộ lọc.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def selectFirstRows(employees: pd.DataFrame) -> pd.DataFrame:
    return employees.head(3)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
