---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2885. Rename Columns](https://leetcode.com/problems/rename-columns)

[中文文档](/solution/2800-2899/2885.Rename%20Columns/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>students</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| id          | int    |
| first       | object |
| last        | object |
| age         | int    |
+-------------+--------+
</pre>

<p>Hãy viết lời giải để đổi tên các cột như sau:</p>

<ul>
	<li><code>id</code> thành <code>student_id</code></li>
	<li><code>first</code> thành <code>first_name</code></li>
	<li><code>last</code> thành <code>last_name</code></li>
	<li><code>age</code> thành <code>age_in_years</code></li>
</ul>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<pre>
<strong class="example">Ví dụ 1:</strong>
<strong>Đầu vào:
</strong>+----+---------+----------+-----+
| id | first   | last     | age |
+----+---------+----------+-----+
| 1  | Mason   | King     | 6   |
| 2  | Ava     | Wright   | 7   |
| 3  | Taylor  | Hall     | 16  |
| 4  | Georgia | Thompson | 18  |
| 5  | Thomas  | Moore    | 10  |
+----+---------+----------+-----+
<strong>Đầu ra:</strong>
+------------+------------+-----------+--------------+
| student_id | first_name | last_name | age_in_years |
+------------+------------+-----------+--------------+
| 1          | Mason      | King      | 6            |
| 2          | Ava        | Wright    | 7            |
| 3          | Taylor     | Hall      | 16           |
| 4          | Georgia    | Thompson  | 18           |
| 5          | Thomas     | Moore     | 10           |
+------------+------------+-----------+--------------+
<strong>Giải thích:</strong>
Tên các cột được thay đổi tương ứng.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần ánh xạ lại bốn tên cột. Một lệnh `rename(columns=...)` duy nhất áp dụng mapping mà không dựa vào thứ tự vị trí.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def renameColumns(students: pd.DataFrame) -> pd.DataFrame:
    students.rename(
        columns={
            'id': 'student_id',
            'first': 'first_name',
            'last': 'last_name',
            'age': 'age_in_years',
        },
        inplace=True,
    )
    return students
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
