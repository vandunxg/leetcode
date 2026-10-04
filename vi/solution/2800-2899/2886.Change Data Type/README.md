---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2886. Change Data Type](https://leetcode.com/problems/change-data-type)

[中文文档](/solution/2800-2899/2886.Change%20Data%20Type/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>students</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
| grade       | float  |
+-------------+--------+
</pre>

<p>Viết lời giải để sửa các lỗi:</p>

<p>Cột <code>grade</code> được lưu dưới dạng số thực, hãy chuyển nó thành số nguyên.</p>

<p>Định dạng kết quả như trong ví dụ dưới đây.</p>

<p>&nbsp;</p>
<pre>
<strong class="example">Ví dụ 1:</strong>
<strong>Đầu vào:
</strong>DataFrame students:
+------------+------+-----+-------+
| student_id | name | age | grade |
+------------+------+-----+-------+
| 1          | Ava  | 6   | 73.0  |
| 2          | Kate | 15  | 87.0  |
+------------+------+-----+-------+
<strong>Đầu ra:
</strong>+------------+------+-----+-------+
| student_id | name | age | grade |
+------------+------+-----+-------+
| 1          | Ava  | 6   | 73    |
| 2          | Kate | 15  | 87    |
+------------+------+-----+-------+
<strong>Giải thích:</strong>
Kiểu dữ liệu của cột grade được chuyển thành int.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> `grade` được lưu dưới dạng số thực và phải chuyển thành số nguyên. `astype(int)` chuyển đổi cột đó mà không làm thay đổi các giá trị số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def changeDatatype(students: pd.DataFrame) -> pd.DataFrame:
    students['grade'] = students['grade'].astype(int)
    return students
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
