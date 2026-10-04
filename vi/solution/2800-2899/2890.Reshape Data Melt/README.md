---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2890. Reshape Data Melt](https://leetcode.com/problems/reshape-data-melt)

[中文文档](/solution/2800-2899/2890.Reshape%20Data%20Melt/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>report</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| product     | object |
| quarter_1   | int    |
| quarter_2   | int    |
| quarter_3   | int    |
| quarter_4   | int    |
+-------------+--------+
</pre>

<p>Viết lời giải để <strong>reshape</strong> dữ liệu sao cho mỗi hàng biểu diễn dữ liệu doanh số của một sản phẩm trong một quý cụ thể.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>+-------------+-----------+-----------+-----------+-----------+
| product     | quarter_1 | quarter_2 | quarter_3 | quarter_4 |
+-------------+-----------+-----------+-----------+-----------+
| Umbrella    | 417       | 224       | 379       | 611       |
| SleepingBag | 800       | 936       | 93        | 875       |
+-------------+-----------+-----------+-----------+-----------+
<strong>Đầu ra:</strong>
+-------------+-----------+-------+
| product     | quarter   | sales |
+-------------+-----------+-------+
| Umbrella    | quarter_1 | 417   |
| SleepingBag | quarter_1 | 800   |
| Umbrella    | quarter_2 | 224   |
| SleepingBag | quarter_2 | 936   |
| Umbrella    | quarter_3 | 379   |
| SleepingBag | quarter_3 | 93    |
| Umbrella    | quarter_4 | 611   |
| SleepingBag | quarter_4 | 875   |
+-------------+-----------+-------+
<strong>Giải thích:</strong>
DataFrame được reshape từ dạng wide sang dạng long. Mỗi hàng biểu diễn doanh số của một sản phẩm trong một quý.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các cột quý cần được gộp thành tên quý và giá trị doanh số. `melt` giữ `product` làm cột định danh và unpivot các cột còn lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def meltTable(report: pd.DataFrame) -> pd.DataFrame:
    return pd.melt(report, id_vars=['product'], var_name='quarter', value_name='sales')
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
