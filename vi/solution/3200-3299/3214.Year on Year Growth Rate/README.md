---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3214. Year on Year Growth Rate 🔒](https://leetcode.com/problems/year-on-year-growth-rate)

[中文文档](/solution/3200-3299/3214.Year%20on%20Year%20Growth%20Rate/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>user_transactions</code></p>

<pre>
+------------------+----------+
| Column Name      | Type     |
+------------------+----------+
| transaction_id   | integer  |
| product_id       | integer  |
| spend            | decimal  |
| transaction_date | datetime |
+------------------+----------+
Cột transaction_id xác định duy nhất mỗi hàng trong bảng này.
Mỗi hàng trong bảng chứa ID giao dịch, ID sản phẩm, số tiền chi tiêu và ngày giao dịch.
</pre>

<p>Hãy viết lời giải để tính <strong>tỷ lệ tăng trưởng theo năm</strong> của tổng chi tiêu <strong>cho từng sản phẩm</strong>.</p>

<p>Bảng kết quả cần bao gồm các cột sau:</p>

<ul>
	<li><code>year</code>: Năm giao dịch.</li>
	<li><code>product_id</code>: ID của sản phẩm.</li>
	<li><code>curr_year_spend</code>: Tổng chi tiêu trong năm hiện tại.</li>
	<li><code>prev_year_spend</code>: Tổng chi tiêu trong năm trước.</li>
	<li><code>yoy_rate</code>: Phần trăm tỷ lệ tăng trưởng theo năm, làm tròn đến <code>2</code> chữ số thập phân.</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em>&nbsp;<code>product_id</code>,<code>year</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>user_transactions</code>:</p>

<pre class="example-io">
+----------------+------------+---------+---------------------+
| transaction_id | product_id | spend   | transaction_date    |
+----------------+------------+---------+---------------------+
| 1341           | 123424     | 1500.60 | 2019-12-31 12:00:00 |
| 1423           | 123424     | 1000.20 | 2020-12-31 12:00:00 |
| 1623           | 123424     | 1246.44 | 2021-12-31 12:00:00 |
| 1322           | 123424     | 2145.32 | 2022-12-31 12:00:00 |
+----------------+------------+---------+---------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------+------------+----------------+----------------+----------+
| year | product_id | curr_year_spend| prev_year_spend| yoy_rate |
+------+------------+----------------+----------------+----------+
| 2019 | 123424     | 1500.60        | NULL           | NULL     |
| 2020 | 123424     | 1000.20        | 1500.60        | -33.35   |
| 2021 | 123424     | 1246.44        | 1000.20        | 24.62    |
| 2022 | 123424     | 2145.32        | 1246.44        | 72.12    |
+------+------------+----------------+----------------+----------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đối với sản phẩm ID 123424:
	<ul>
		<li>Năm 2019:
		<ul>
			<li>Chi tiêu trong năm hiện tại là 1500.60</li>
			<li>Không có chi tiêu nào được ghi nhận trong năm trước</li>
			<li>Tỷ lệ tăng trưởng theo năm: NULL</li>
		</ul>
		</li>
		<li>Năm 2020:
		<ul>
			<li>Chi tiêu trong năm hiện tại là 1000.20</li>
			<li>Chi tiêu trong năm trước là 1500.60</li>
			<li>Tỷ lệ tăng trưởng theo năm: ((1000.20 - 1500.60) / 1500.60) * 100 = -33.35%</li>
		</ul>
		</li>
		<li>Năm 2021:
		<ul>
			<li>Chi tiêu trong năm hiện tại là 1246.44</li>
			<li>Chi tiêu trong năm trước là 1000.20</li>
			<li>Tỷ lệ tăng trưởng theo năm: ((1246.44 - 1000.20) / 1000.20) * 100 = 24.62%</li>
		</ul>
		</li>
		<li>Năm 2022:
		<ul>
			<li>Chi tiêu trong năm hiện tại là 2145.32</li>
			<li>Chi tiêu trong năm trước là 1246.44</li>
			<li>Tỷ lệ tăng trưởng theo năm: ((2145.32 - 1246.44) / 1246.44) * 100 = 72.12%</li>
		</ul>
		</li>
	</ul>
	</li>
</ul>

<p><strong>Lưu ý:</strong> Bảng kết quả được sắp xếp theo <code>product_id</code> và <code>year</code> theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thống kê theo nhóm + Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Cần tính tổng chi tiêu theo sản phẩm và năm, sau đó ghép mỗi năm với năm trước. Có thể dùng vòng lặp group ở phía application, nhưng các phép toán trên tập hợp trực tiếp hơn.
>
> Trước tiên, nhóm theo `product_id` và `YEAR(transaction_date)` để tính chi tiêu trong năm hiện tại, sau đó left join với dòng của cùng sản phẩm có năm nhỏ hơn 1 và tính $(\textit{curr}-\textit{prev})/\textit{prev}$. Những năm không có năm trước sẽ giữ giá trị null.

<!-- thinking:end -->

Trước tiên, ta có thể nhóm theo `product_id` và `year(transaction_date)` để thống kê, sau đó dùng left join để liên kết số liệu của năm hiện tại với số liệu của năm trước, cuối cùng tính tỷ lệ tăng trưởng theo năm.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT product_id, YEAR(transaction_date) year, SUM(spend) curr_year_spend
        FROM user_transactions
        GROUP BY 1, 2
    ),
    S AS (
        SELECT t1.year, t1.product_id, t1.curr_year_spend, t2.curr_year_spend prev_year_spend
        FROM
            T t1
            LEFT JOIN T t2 ON t1.product_id = t2.product_id AND t1.year = t2.year + 1
    )
SELECT
    *,
    ROUND((curr_year_spend - prev_year_spend) / prev_year_spend * 100, 2) yoy_rate
FROM S
ORDER BY 2, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
