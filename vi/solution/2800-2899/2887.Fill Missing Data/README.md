---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2887. Fill Missing Data](https://leetcode.com/problems/fill-missing-data)

[中文文档](/solution/2800-2899/2887.Fill%20Missing%20Data/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>products</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| name        | object |
| quantity    | int    |
| price       | int    |
+-------------+--------+
</pre>

<p>Viết lời giải để điền giá trị còn thiếu trong cột <code>quantity</code> bằng <code><strong>0</strong></code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<pre>
<strong class="example">Ví dụ 1:</strong>
<strong>Đầu vào:</strong>+-----------------+----------+-------+
| name            | quantity | price |
+-----------------+----------+-------+
| Wristwatch      | None     | 135   |
| WirelessEarbuds | None     | 821   |
| GolfClubs       | 779      | 9319  |
| Printer         | 849      | 3051  |
+-----------------+----------+-------+
<strong>Đầu ra:
</strong>+-----------------+----------+-------+
| name            | quantity | price |
+-----------------+----------+-------+
| Wristwatch      | 0        | 135   |
| WirelessEarbuds | 0        | 821   |
| GolfClubs       | 779      | 9319  |
| Printer         | 849      | 3051  |
+-----------------+----------+-------+
<strong>Giải thích:</strong>
quantity của Wristwatch và WirelessEarbuds được điền bằng 0.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị rỗng trong quantity cần được chuyển thành $0$. `fillna(0)` trên cột này sẽ giữ nguyên các giá trị còn thiếu ở những cột khác.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def fillMissingValues(products: pd.DataFrame) -> pd.DataFrame:
    products['quantity'] = products['quantity'].fillna(0)
    return products
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
