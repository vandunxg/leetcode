---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2889. Reshape Data Pivot](https://leetcode.com/problems/reshape-data-pivot)

[中文文档](/solution/2800-2899/2889.Reshape%20Data%20Pivot/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>weather</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| city        | object |
| month       | object |
| temperature | int    |
+-------------+--------+
</pre>

<p>Hãy viết lời giải để <strong>pivot</strong> dữ liệu sao cho mỗi hàng thể hiện nhiệt độ của một tháng cụ thể và mỗi thành phố là một cột riêng.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<pre>
<strong class="example">Ví dụ 1:</strong>
<strong>Đầu vào:</strong>
+--------------+----------+-------------+
| city         | month    | temperature |
+--------------+----------+-------------+
| Jacksonville | January  | 13          |
| Jacksonville | February | 23          |
| Jacksonville | March    | 38          |
| Jacksonville | April    | 5           |
| Jacksonville | May      | 34          |
| ElPaso       | January  | 20          |
| ElPaso       | February | 6           |
| ElPaso       | March    | 26          |
| ElPaso       | April    | 2           |
| ElPaso       | May      | 43          |
+--------------+----------+-------------+
<strong>Đầu ra:</strong><code>
+----------+--------+--------------+
| month    | ElPaso | Jacksonville |
+----------+--------+--------------+
| April    | 2      | 5            |
| February | 6      | 23           |
| January  | 20     | 13           |
| March    | 26     | 38           |
| May      | 43     | 34           |
+----------+--------+--------------+</code>
<strong>Giải thích:
</strong>Bảng được pivot, mỗi cột thể hiện một thành phố và mỗi hàng thể hiện một tháng cụ thể.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các tháng nên trở thành chỉ số, các thành phố là các cột, còn nhiệt độ là các giá trị. `pivot` định hình lại ba trường đó, trong đó mỗi cặp (month, city) chứa một nhiệt độ duy nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def pivotTable(weather: pd.DataFrame) -> pd.DataFrame:
    return weather.pivot(index='month', columns='city', values='temperature')
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
