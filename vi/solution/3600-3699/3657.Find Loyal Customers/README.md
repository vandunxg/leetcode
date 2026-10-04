---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3657. Find Loyal Customers](https://leetcode.com/problems/find-loyal-customers)

[Tài liệu tiếng Trung](/solution/3600-3699/3657.Find%20Loyal%20Customers/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>customer_transactions</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| transaction_id   | int     |
| customer_id      | int     |
| transaction_date | date    |
| amount           | decimal |
| transaction_type | varchar |
+------------------+---------+
transaction_id is the unique identifier for this table.
transaction_type can be either &#39;purchase&#39; or &#39;refund&#39;.
</pre>

<p>Hãy viết lời giải để tìm <strong>khách hàng trung thành</strong>. Một khách hàng được xem là <strong>trung thành</strong> nếu thỏa mãn TẤT CẢ các tiêu chí sau:</p>

<ul>
	<li>Đã thực hiện <strong>ít nhất</strong>&nbsp;<code><font face="monospace">3</font></code>&nbsp; giao dịch mua hàng.</li>
	<li>Đã hoạt động trong <strong>ít nhất</strong> <code>30</code> ngày.</li>
	<li><strong>Tỷ lệ hoàn tiền</strong> nhỏ hơn <code>20%</code> .</li>
</ul>

<p><em>Tỷ lệ hoàn tiền</em> là tỷ lệ giữa số giao dịch hoàn tiền và tổng số giao dịch (mua hàng cộng hoàn tiền).</p>

<p>Trả về <em>bảng kết quả&nbsp;được sắp xếp theo</em> <code>customer_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng customer_transactions:</p>

<pre class="example-io">
+----------------+-------------+------------------+--------+------------------+
| transaction_id | customer_id | transaction_date | amount | transaction_type |
+----------------+-------------+------------------+--------+------------------+
| 1              | 101         | 2024-01-05       | 150.00 | purchase         |
| 2              | 101         | 2024-01-15       | 200.00 | purchase         |
| 3              | 101         | 2024-02-10       | 180.00 | purchase         |
| 4              | 101         | 2024-02-20       | 250.00 | purchase         |
| 5              | 102         | 2024-01-10       | 100.00 | purchase         |
| 6              | 102         | 2024-01-12       | 120.00 | purchase         |
| 7              | 102         | 2024-01-15       | 80.00  | refund           |
| 8              | 102         | 2024-01-18       | 90.00  | refund           |
| 9              | 102         | 2024-02-15       | 130.00 | purchase         |
| 10             | 103         | 2024-01-01       | 500.00 | purchase         |
| 11             | 103         | 2024-01-02       | 450.00 | purchase         |
| 12             | 103         | 2024-01-03       | 400.00 | purchase         |
| 13             | 104         | 2024-01-01       | 200.00 | purchase         |
| 14             | 104         | 2024-02-01       | 250.00 | purchase         |
| 15             | 104         | 2024-02-15       | 300.00 | purchase         |
| 16             | 104         | 2024-03-01       | 350.00 | purchase         |
| 17             | 104         | 2024-03-10       | 280.00 | purchase         |
| 18             | 104         | 2024-03-15       | 100.00 | refund           |
+----------------+-------------+------------------+--------+------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+
| customer_id |
+-------------+
| 101         |
| 104         |
+-------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Khách hàng 101</strong>:

    <ul>
       <li>Các giao dịch mua hàng: 4 (ID: 1, 2, 3, 4)&nbsp;</li>
       <li>Các giao dịch hoàn tiền: 0</li>
       <li>Tỷ lệ hoàn tiền: 0/4 = 0% (nhỏ hơn 20%)&nbsp;</li>
       <li>Thời gian hoạt động: 5 tháng 1 đến 20 tháng 2 = 46 ngày (ít nhất 30 ngày)&nbsp;</li>
       <li>Đủ điều kiện là khách hàng trung thành&nbsp;</li>
    </ul>
    </li>
    <li><strong>Khách hàng 102</strong>:
    <ul>
       <li>Các giao dịch mua hàng: 3 (ID: 5, 6, 9)&nbsp;</li>
       <li>Các giao dịch hoàn tiền: 2 (ID: 7, 8)</li>
       <li>Tỷ lệ hoàn tiền: 2/5 = 40% (vượt quá 20%)&nbsp;</li>
       <li>Không trung thành&nbsp;</li>
    </ul>
    </li>
    <li><strong>Khách hàng 103</strong>:
    <ul>
       <li>Các giao dịch mua hàng: 3 (ID: 10, 11, 12)&nbsp;</li>
       <li>Các giao dịch hoàn tiền: 0</li>
       <li>Tỷ lệ hoàn tiền: 0/3 = 0% (nhỏ hơn 20%)&nbsp;</li>
       <li>Thời gian hoạt động: 1 tháng 1 đến 3 tháng 1 = 2 ngày (ít hơn 30 ngày)&nbsp;</li>
       <li>Không trung thành&nbsp;</li>
    </ul>
    </li>
    <li><strong>Khách hàng 104</strong>:
    <ul>
       <li>Các giao dịch mua hàng: 5 (ID: 13, 14, 15, 16, 17)&nbsp;</li>
       <li>Các giao dịch hoàn tiền: 1 (ID: 18)</li>
       <li>Tỷ lệ hoàn tiền: 1/6 = 16.67% (nhỏ hơn 20%)&nbsp;</li>
       <li>Thời gian hoạt động: 1 tháng 1 đến 15 tháng 3 = 73 ngày (ít nhất 30 ngày)&nbsp;</li>
       <li>Đủ điều kiện là khách hàng trung thành&nbsp;</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo customer_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lòng trung thành phụ thuộc vào số lượng giao dịch, tỷ lệ hoàn tiền và khoảng thời gian hoạt động, vì vậy chỉ cần thực hiện một phép gom nhóm theo $\textit{customer\_id}$ là đủ.
>
> Tính tổng số giao dịch, số giao dịch hoàn tiền, ngày đầu tiên và ngày cuối cùng. Khoảng thời gian là số ngày chênh lệch; tỷ lệ hoàn tiền là số giao dịch hoàn tiền chia cho tổng số giao dịch.
>
> Giữ lại những khách hàng có ít nhất ba giao dịch, tỷ lệ hoàn tiền nhỏ hơn $0.2$ và khoảng thời gian hoạt động ít nhất $30$ ngày, sau đó sắp xếp theo id.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT customer_id
FROM customer_transactions
GROUP BY 1
HAVING
    COUNT(1) >= 3
    AND SUM(transaction_type = 'refund') / COUNT(1) < 0.2
    AND DATEDIFF(MAX(transaction_date), MIN(transaction_date)) >= 30
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def find_loyal_customers(customer_transactions: pd.DataFrame) -> pd.DataFrame:
    customer_transactions["transaction_date"] = pd.to_datetime(
        customer_transactions["transaction_date"]
    )
    grouped = customer_transactions.groupby("customer_id")
    agg_df = grouped.agg(
        total_transactions=("transaction_type", "size"),
        refund_count=("transaction_type", lambda x: (x == "refund").sum()),
        min_date=("transaction_date", "min"),
        max_date=("transaction_date", "max"),
    ).reset_index()
    agg_df["date_diff"] = (agg_df["max_date"] - agg_df["min_date"]).dt.days
    agg_df["refund_ratio"] = agg_df["refund_count"] / agg_df["total_transactions"]
    result = (
        agg_df[
            (agg_df["total_transactions"] >= 3)
            & (agg_df["refund_ratio"] < 0.2)
            & (agg_df["date_diff"] >= 30)
        ][["customer_id"]]
        .sort_values("customer_id")
        .reset_index(drop=True)
    )
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
