---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2880. Select Data](https://leetcode.com/problems/select-data)

[中文文档](/solution/2800-2899/2880.Select%20Data/README.md)

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

<p>Hãy viết lời giải để chọn tên và tuổi của học sinh có <code>student_id = 101</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<pre>
<strong>Ví dụ 1:
Đầu vào:</strong>
+------------+---------+-----+
| student_id | name    | age |
+------------+---------+-----+
| 101        | Ulysses | 13  |
| 53         | William | 10  |
| 128        | Henry   | 6   |
| 3          | Henry   | 11  |
+------------+---------+-----+
<strong>Đầu ra:</strong>
+---------+-----+
| name    | age |
+---------+-----+
| Ulysses | 13  |
+---------+-----+
<strong>Giải thích:
</strong>Học sinh Ulysses có student_id = 101, nên chúng ta chọn tên và tuổi của học sinh này.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần lấy tên và tuổi của học sinh $101$. Một boolean mask sẽ chọn hàng đó, sau đó chỉ giữ lại hai cột `name` và `age`.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def selectData(students: pd.DataFrame) -> pd.DataFrame:
    return students[students['student_id'] == 101][['name', 'age']]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
