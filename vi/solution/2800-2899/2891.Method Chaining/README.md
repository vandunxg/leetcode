---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2891. Method Chaining](https://leetcode.com/problems/method-chaining)

[中文文档](/solution/2800-2899/2891.Method%20Chaining/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>animals</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| name        | object |
| species     | object |
| age         | int    |
| weight      | int    |
+-------------+--------+
</pre>

<p>Hãy viết lời giải để liệt kê tên các động vật có cân nặng <strong>lớn hơn</strong> <code>100</code> kilogram.</p>

<p>Trả về các động vật được sắp xếp theo cân nặng <strong>giảm dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
DataFrame animals:
+----------+---------+-----+--------+
| name     | species | age | weight |
+----------+---------+-----+--------+
| Tatiana  | Snake   | 98  | 464    |
| Khaled   | Giraffe | 50  | 41     |
| Alex     | Leopard | 6   | 328    |
| Jonathan | Monkey  | 45  | 463    |
| Stefan   | Bear    | 100 | 50     |
| Tommy    | Panda   | 26  | 349    |
+----------+---------+-----+--------+
<strong>Đầu ra:</strong>
+----------+
| name     |
+----------+
| Tatiana  |
| Jonathan |
| Tommy    |
| Alex     |
+----------+
<strong>Giải thích:</strong>
Tất cả động vật có cân nặng lớn hơn 100 đều phải được đưa vào bảng kết quả.
Cân nặng của Tatiana là 464, Jonathan là 463, Tommy là 349 và Alex là 328.
Kết quả phải được sắp xếp theo cân nặng giảm dần.</pre>

<p>&nbsp;</p>
<p>Trong Pandas, <strong>method chaining</strong> cho phép chúng ta thực hiện các thao tác trên DataFrame mà không cần tách từng thao tác thành một dòng riêng hoặc tạo nhiều biến tạm thời.</p>

<p>Bạn có thể hoàn thành task này chỉ bằng <strong>một dòng</strong> code sử dụng method chaining không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Giữ lại các động vật nặng hơn $100$, sắp xếp theo cân nặng giảm dần, rồi chỉ lấy cột name. Method chaining thực hiện việc lọc, sắp xếp và chọn cột trong cùng một biểu thức.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def findHeavyAnimals(animals: pd.DataFrame) -> pd.DataFrame:
    return animals[animals['weight'] > 100].sort_values('weight', ascending=False)[
        ['name']
    ]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
