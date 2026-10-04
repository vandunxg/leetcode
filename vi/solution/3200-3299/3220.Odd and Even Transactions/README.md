---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3220. Odd and Even Transactions](https://leetcode.com/problems/odd-and-even-transactions)

[中文文档](/solution/3200-3299/3220.Odd%20and%20Even%20Transactions/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>transactions</code></p>

<pre>
+------------------+------+
| Column Name      | Type |
+------------------+------+
| transaction_id   | int  |
| amount           | int  |
| transaction_date | date |
+------------------+------+
Cột transactions_id định danh duy nhất cho mỗi hàng trong bảng này.
Mỗi hàng trong bảng này chứa mã giao dịch, số tiền và ngày giao dịch.
</pre>

<p>Viết lời giải để tìm <strong>tổng số tiền</strong> của các giao dịch <strong>lẻ</strong> và <strong>chẵn</strong> cho mỗi ngày. Nếu một ngày cụ thể không có giao dịch lẻ hoặc chẵn, hãy hiển thị <code>0</code>.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>transaction_date</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>transactions</code>:</p>

<pre class="example-io">
+----------------+--------+------------------+
| transaction_id | amount | transaction_date |
+----------------+--------+------------------+
| 1              | 150    | 2024-07-01       |
| 2              | 200    | 2024-07-01       |
| 3              | 75     | 2024-07-01       |
| 4              | 300    | 2024-07-02       |
| 5              | 50     | 2024-07-02       |
| 6              | 120    | 2024-07-03       |
+----------------+--------+------------------+
  </pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------------+---------+----------+
| transaction_date | odd_sum | even_sum |
+------------------+---------+----------+
| 2024-07-01       | 75      | 350      |
| 2024-07-02       | 0       | 350      |
| 2024-07-03       | 0       | 120      |
+------------------+---------+----------+
  </pre>

<p><strong>Giải thích:</strong></p>

<ul>
     <li>Với các ngày giao dịch:
     <ul>
         <li>2024-07-01:
         <ul>
             <li>Tổng số tiền của các giao dịch lẻ: 75</li>
             <li>Tổng số tiền của các giao dịch chẵn: 150 + 200 = 350</li>
         </ul>
         </li>
         <li>2024-07-02:
         <ul>
             <li>Tổng số tiền của các giao dịch lẻ: 0</li>
             <li>Tổng số tiền của các giao dịch chẵn: 300 + 50 = 350</li>
         </ul>
         </li>
         <li>2024-07-03:
         <ul>
             <li>Tổng số tiền của các giao dịch lẻ: 0</li>
             <li>Tổng số tiền của các giao dịch chẵn: 120</li>
         </ul>
         </li>
     </ul>
     </li>
</ul>

<p><strong>Lưu ý:</strong> Bảng kết quả được sắp xếp theo <code>transaction_date</code> theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần tính tổng các khoản tiền lẻ và chẵn theo từng ngày. Có thể duyệt theo từng ngày, nhưng tách tính chẵn lẻ thành hai cột rồi nhóm sẽ rõ ràng hơn.
>
> Giữ lại `amount` khi nó là số lẻ hoặc chẵn, và ghi $0$ trong trường hợp còn lại, sau đó tính tổng theo `transaction_date` và sắp xếp tăng dần. Tính chẵn lẻ trên từng hàng; một phép tổng hợp là đủ để hoàn thành truy vấn.

<!-- thinking:end -->

Chúng ta có thể nhóm dữ liệu theo `transaction_date`, sau đó tính riêng tổng số tiền của các giao dịch lẻ và chẵn. Cuối cùng, sắp xếp theo `transaction_date` theo thứ tự tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    transaction_date,
    SUM(IF(amount % 2 = 1, amount, 0)) AS odd_sum,
    SUM(IF(amount % 2 = 0, amount, 0)) AS even_sum
FROM transactions
GROUP BY 1
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def sum_daily_odd_even(transactions: pd.DataFrame) -> pd.DataFrame:
    transactions["odd_sum"] = transactions["amount"].where(
        transactions["amount"] % 2 == 1, 0
    )
    transactions["even_sum"] = transactions["amount"].where(
        transactions["amount"] % 2 == 0, 0
    )

    result = (
        transactions.groupby("transaction_date")
        .agg(odd_sum=("odd_sum", "sum"), even_sum=("even_sum", "sum"))
        .reset_index()
    )

    result = result.sort_values("transaction_date")

    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
