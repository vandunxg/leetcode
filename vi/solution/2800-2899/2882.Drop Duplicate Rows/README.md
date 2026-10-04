---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2882. Drop Duplicate Rows](https://leetcode.com/problems/drop-duplicate-rows)

[中文文档](/solution/2800-2899/2882.Drop%20Duplicate%20Rows/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame customers
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| customer_id | int    |
| name        | object |
| email       | object |
+-------------+--------+
</pre>

<p>DataFrame có một số hàng trùng lặp dựa trên cột <code>email</code>.</p>

<p>Hãy viết lời giải để xóa các hàng trùng lặp này và chỉ giữ lại lần xuất hiện <strong>đầu tiên</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<pre>
<strong class="example">Ví dụ 1:</strong>
<strong>Đầu vào:</strong>
+-------------+---------+---------------------+
| customer_id | name    | email               |
+-------------+---------+---------------------+
| 1           | Ella    | emily@example.com   |
| 2           | David   | michael@example.com |
| 3           | Zachary | sarah@example.com   |
| 4           | Alice   | john@example.com    |
| 5           | Finn    | john@example.com    |
| 6           | Violet  | alice@example.com   |
+-------------+---------+---------------------+
<strong>Đầu ra: </strong>
+-------------+---------+---------------------+
| customer_id | name    | email               |
+-------------+---------+---------------------+
| 1           | Ella    | emily@example.com   |
| 2           | David   | michael@example.com |
| 3           | Zachary | sarah@example.com   |
| 4           | Alice   | john@example.com    |
| 6           | Violet  | alice@example.com   |
+-------------+---------+---------------------+
<strong>Giải thích:</strong>
Alic (customer_id = 4) và Finn (customer_id = 5) đều sử dụng john@example.com, vì vậy chỉ giữ lại lần xuất hiện đầu tiên của email này.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng trùng lặp được xác định dựa trên `email`. `drop_duplicates(subset=['email'])` giữ lại lần xuất hiện đầu tiên của mỗi địa chỉ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def dropDuplicateEmails(customers: pd.DataFrame) -> pd.DataFrame:
    return customers.drop_duplicates(subset=['email'])
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
