---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2888. Reshape Data Concatenate](https://leetcode.com/problems/reshape-data-concatenate)

[中文文档](/solution/2800-2899/2888.Reshape%20Data%20Concatenate/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>df1</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
+-------------+--------+

DataFrame <code>df2</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
+-------------+--------+

</pre>

<p>Viết lời giải để nối hai DataFrame này theo <strong>chiều dọc</strong> thành một DataFrame.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
df1</strong>
+------------+---------+-----+
| student_id | name    | age |
+------------+---------+-----+
| 1          | Mason   | 8   |
| 2          | Ava     | 6   |
| 3          | Taylor  | 15  |
| 4          | Georgia | 17  |
+------------+---------+-----+
<strong>df2
</strong>+------------+------+-----+
| student_id | name | age |
+------------+------+-----+
| 5          | Leo  | 7   |
| 6          | Alex | 7   |
+------------+------+-----+
<strong>Đầu ra:</strong>
+------------+---------+-----+
| student_id | name    | age |
+------------+---------+-----+
| 1          | Mason   | 8   |
| 2          | Ava     | 6   |
| 3          | Taylor  | 15  |
| 4          | Georgia | 17  |
| 5          | Leo     | 7   |
| 6          | Alex    | 7   |
+------------+---------+-----+
<strong>Giải thích:
</strong>Hai DataFrame được xếp chồng theo chiều dọc và các hàng của chúng được gộp lại.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai DataFrame có cùng các cột và cần được xếp chồng. `concat` với `ignore_index=True` tạo lại chỉ số thay vì giữ các nhãn hàng ban đầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def concatenateTables(df1: pd.DataFrame, df2: pd.DataFrame) -> pd.DataFrame:
    return pd.concat([df1, df2], ignore_index=True)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
