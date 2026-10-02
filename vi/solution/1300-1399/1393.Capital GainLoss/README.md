---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1393. Capital GainLoss](https://leetcode.com/problems/capital-gainloss)

[中文文档](/solution/1300-1399/1393.Capital%20GainLoss/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Stocks</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| stock_name    | varchar |
| operation     | enum    |
| operation_day | int     |
| price         | int     |
+---------------+---------+
(stock_name, operation_day) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Cột operation là một ENUM (danh mục) có kiểu (&#39;Sell&#39;, &#39;Buy&#39;)
Mỗi hàng trong bảng cho biết cổ phiếu có tên stock_name đã thực hiện giao dịch với mức giá price vào ngày operation_day.
Đảm bảo mỗi giao dịch &#39;Sell&#39; của một cổ phiếu đều có giao dịch &#39;Buy&#39; tương ứng vào một ngày trước đó. Đồng thời, mỗi giao dịch &#39;Buy&#39; của một cổ phiếu đều có giao dịch &#39;Sell&#39; tương ứng vào một ngày sau đó.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tính <strong>lãi/lỗ vốn</strong> cho từng cổ phiếu.</p>

<p><strong>Lãi/lỗ vốn</strong> của một cổ phiếu là tổng số tiền lãi hoặc lỗ sau một hay nhiều lần mua và bán cổ phiếu đó.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Stocks:
+---------------+-----------+---------------+--------+
| stock_name    | operation | operation_day | price  |
+---------------+-----------+---------------+--------+
| Leetcode      | Buy       | 1             | 1000   |
| Corona Masks  | Buy       | 2             | 10     |
| Leetcode      | Sell      | 5             | 9000   |
| Handbags      | Buy       | 17            | 30000  |
| Corona Masks  | Sell      | 3             | 1010   |
| Corona Masks  | Buy       | 4             | 1000   |
| Corona Masks  | Sell      | 5             | 500    |
| Corona Masks  | Buy       | 6             | 1000   |
| Handbags      | Sell      | 29            | 7000   |
| Corona Masks  | Sell      | 10            | 10000  |
+---------------+-----------+---------------+--------+
<strong>Đầu ra:</strong> 
+---------------+-------------------+
| stock_name    | capital_gain_loss |
+---------------+-------------------+
| Corona Masks  | 9500              |
| Leetcode      | 8000              |
| Handbags      | -23000            |
+---------------+-------------------+
<strong>Giải thích:</strong> 
Cổ phiếu Leetcode được mua vào ngày 1 với giá 1000$ và bán vào ngày 5 với giá 9000$. Lãi vốn = 9000 - 1000 = 8000$.
Cổ phiếu Handbags được mua vào ngày 17 với giá 30000$ và bán vào ngày 29 với giá 7000$. Lỗ vốn = 7000 - 30000 = -23000$.
Cổ phiếu Corona Masks được mua vào ngày 1 với giá 10$ và bán vào ngày 3 với giá 1010$. Cổ phiếu được mua lại vào ngày 4 với giá 1000$ rồi bán vào ngày 5 với giá 500$. Cuối cùng, cổ phiếu được mua vào ngày 6 với giá 1000$ và bán vào ngày 10 với giá 10000$. Lãi/lỗ vốn là tổng lãi/lỗ của từng giao dịch (&#39;Buy&#39; --&gt; &#39;Sell&#39;) = (1010 - 10) + (500 - 1000) + (10000 - 1000) = 1000 - 500 + 9000 = 9500$.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: GROUP BY + SUM(IF())

<!-- thinking:start -->

> **Tư duy**
>
> Lãi/lỗ vốn ròng của mỗi cổ phiếu bằng tổng giá bán trừ tổng giá mua. Nhóm theo $\textit{stock\_name}$ rồi tính $\mathrm{SUM}(\mathrm{IF}(\textit{operation}=\texttt{'Buy'},-\textit{price},\textit{price}))$ sẽ cho kết quả ròng chỉ trong một lượt.

<!-- thinking:end -->

Ta dùng `GROUP BY` để nhóm các giao dịch mua và bán của cùng một cổ phiếu, sau đó dùng `SUM(IF())` để tính lãi/lỗ vốn của từng cổ phiếu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    stock_name,
    SUM(IF(operation = 'Buy', -price, price)) AS capital_gain_loss
FROM Stocks
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
