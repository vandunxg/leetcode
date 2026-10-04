---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2881. Create a New Column](https://leetcode.com/problems/create-a-new-column)

[中文文档](/solution/2800-2899/2881.Create%20a%20New%20Column/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>employees</code>
+-------------+--------+
| Column Name | Type.  |
+-------------+--------+
| name        | object |
| salary      | int.   |
+-------------+--------+
</pre>

<p>Một công ty dự định cung cấp tiền thưởng cho nhân viên.</p>

<p>Hãy viết lời giải để tạo một cột mới có tên <code>bonus</code>, chứa <strong>giá trị gấp đôi</strong> của cột <code>salary</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
DataFrame employees
+---------+--------+
| name    | salary |
+---------+--------+
| Piper   | 4548   |
| Grace   | 28150  |
| Georgia | 1103   |
| Willow  | 6593   |
| Finn    | 74576  |
| Thomas  | 24433  |
+---------+--------+
<strong>Đầu ra:</strong>
+---------+--------+--------+
| name    | salary | bonus  |
+---------+--------+--------+
| Piper   | 4548   | 9096   |
| Grace   | 28150  | 56300  |
| Georgia | 1103   | 2206   |
| Willow  | 6593   | 13186  |
| Finn    | 74576  | 149152 |
| Thomas  | 24433  | 48866  |
+---------+--------+--------+
<strong>Giải thích:</strong>
Một cột mới có tên bonus được tạo bằng cách nhân đôi giá trị trong cột salary.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> bonus bằng hai lần salary. Phép gán vector hóa `salary * 2` tạo cột này mà không cần vòng lặp Python.

<!-- thinking:end -->

Ta có thể tính trực tiếp giá trị gấp đôi của `salary`, sau đó lưu kết quả vào cột `bonus`.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def createBonusColumn(employees: pd.DataFrame) -> pd.DataFrame:
    employees['bonus'] = employees['salary'] * 2
    return employees
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
