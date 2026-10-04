---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2877. Create a DataFrame from List](https://leetcode.com/problems/create-a-dataframe-from-list)

[中文文档](/solution/2800-2899/2877.Create%20a%20DataFrame%20from%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết lời giải để <strong>tạo</strong> một DataFrame từ danh sách 2D có tên <code>student_data</code>. Danh sách 2D này chứa ID và tuổi của một số học sinh.</p>

<p>DataFrame phải có hai cột, <code>student_id</code> và <code>age</code>, đồng thời giữ nguyên thứ tự như danh sách 2D ban đầu.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>student_data:<strong>
</strong><code>[
  [1, 15],
  [2, 11],
  [3, 11],
  [4, 20]
]</code>
<strong>Đầu ra:</strong>
+------------+-----+
| student_id | age |
+------------+-----+
| 1          | 15  |
| 2          | 11  |
| 3          | 11  |
| 4          | 20  |
+------------+-----+
<strong>Giải thích:</strong>
Một DataFrame được tạo từ student_data, với hai cột có tên là <code>student_id</code> và <code>age</code>.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Danh sách lồng nhau đã chứa thông tin của mỗi học sinh trên một hàng. Truyền danh sách này cùng với hai tên cột vào `DataFrame` sẽ tạo bảng mà không cần thêm từng hàng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def createDataframe(student_data: List[List[int]]) -> pd.DataFrame:
    return pd.DataFrame(student_data, columns=['student_id', 'age'])
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
