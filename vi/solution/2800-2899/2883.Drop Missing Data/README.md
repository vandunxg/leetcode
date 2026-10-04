---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2883. Drop Missing Data](https://leetcode.com/problems/drop-missing-data)

[中文文档](/solution/2800-2899/2883.Drop%20Missing%20Data/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame students
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
+-------------+--------+
</pre>

<p>Có một số hàng có giá trị bị thiếu trong cột <code>name</code>.</p>

<p>Hãy viết lời giải để xóa các hàng có giá trị bị thiếu.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>+------------+---------+-----+
| student_id | name    | age |
+------------+---------+-----+
| 32         | Piper   | 5   |
| 217        | None    | 19  |
| 779        | Georgia | 20  |
| 849        | Willow  | 14  |
+------------+---------+-----+
<strong>Đầu ra:
</strong>+------------+---------+-----+
| student_id | name    | age |
+------------+---------+-----+
| 32         | Piper   | 5   |
| 779        | Georgia | 20  |
| 849        | Willow  | 14  |
+------------+---------+-----+
<strong>Giải thích:</strong>
Học sinh có id 217 có giá trị trống trong cột name, vì vậy hàng này sẽ bị xóa.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Dữ liệu thiếu nghĩa là `name` có giá trị null. Lọc bằng `notnull()` trên cột đó sẽ loại bỏ các hàng này mà không cần dùng `dropna` cho toàn bộ DataFrame.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def dropMissingData(students: pd.DataFrame) -> pd.DataFrame:
    return students[students['name'].notnull()]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
