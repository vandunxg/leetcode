---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2884. Modify Columns](https://leetcode.com/problems/modify-columns)

[中文文档](/solution/2800-2899/2884.Modify%20Columns/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>employees</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| name        | object |
| salary      | int    |
+-------------+--------+
</pre>

<p>Một công ty dự định tăng lương cho nhân viên.</p>

<p>Hãy viết lời giải để <strong>thay đổi</strong> cột <code>salary</code> bằng cách nhân mỗi mức lương với 2.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>DataFrame employees
+---------+--------+
| name    | salary |
+---------+--------+
| Jack    | 19666  |
| Piper   | 74754  |
| Mia     | 62509  |
| Ulysses | 54866  |
+---------+--------+
<strong>Đầu ra:
</strong>+---------+--------+
| name    | salary |
+---------+--------+
| Jack    | 39332  |
| Piper   | 149508 |
| Mia     | 125018 |
| Ulysses | 109732 |
+---------+--------+
<strong>Giải thích:
</strong>Mỗi mức lương đều được nhân đôi.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi mức lương cần được nhân đôi. Nhân trực tiếp trên cột `salary` sẽ giữ nguyên các phần còn lại của DataFrame.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def modifySalaryColumn(employees: pd.DataFrame) -> pd.DataFrame:
    employees['salary'] *= 2
    return employees
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
