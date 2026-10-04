---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3230. Customer Purchasing Behavior Analysis 🔒](https://leetcode.com/problems/customer-purchasing-behavior-analysis)

[Tài liệu tiếng Trung](/solution/3200-3299/3230.Customer%20Purchasing%20Behavior%20Analysis/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Transactions</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| transaction_id   | int     |
| customer_id      | int     |
| product_id       | int     |
| transaction_date | date    |
| amount           | decimal |
+------------------+---------+
transaction_id là định danh duy nhất của bảng này.
Mỗi hàng trong bảng này chứa thông tin về một giao dịch, bao gồm ID khách hàng, ID sản phẩm, ngày giao dịch và số tiền đã chi.
</pre>

<p>Bảng: <code>Products</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| category    | varchar |
| price       | decimal |
+-------------+---------+
product_id là định danh duy nhất của bảng này.
Mỗi hàng trong bảng này chứa thông tin về một sản phẩm, bao gồm danh mục và giá.
</pre>

<p>Viết lời giải để phân tích hành vi mua hàng của khách hàng. Với <strong>mỗi khách hàng</strong>, hãy tính:</p>

<ul>
	<li>Tổng số tiền đã chi.</li>
	<li>Số lượng giao dịch.</li>
	<li>Số danh mục sản phẩm <strong>khác nhau</strong> đã mua.</li>
	<li>Số tiền trung bình đã chi.&nbsp;</li>
	<li>Danh mục sản phẩm được mua <strong>thường xuyên nhất</strong>&nbsp;(nếu hòa, chọn danh mục có giao dịch gần đây nhất).</li>
	<li><strong>Điểm trung thành</strong>&nbsp;được định nghĩa là: (Số lượng giao dịch * 10) + (Tổng số tiền đã chi / 100).</li>
</ul>

<p>Làm tròn <code>total_amount</code>, <code>avg_transaction_amount</code> và <code>loyalty_score</code> đến <code>2</code> chữ số thập phân.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>loyalty_score</code> <em>theo thứ tự <strong>giảm dần</strong></em>, <em>sau đó theo </em><code>customer_id</code><em> theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>Transactions</code>:</p>

<pre class="example-io">
+----------------+-------------+------------+------------------+--------+
| transaction_id | customer_id | product_id | transaction_date | amount |
+----------------+-------------+------------+------------------+--------+
| 1              | 101         | 1          | 2023-01-01       | 100.00 |
| 2              | 101         | 2          | 2023-01-15       | 150.00 |
| 3              | 102         | 1          | 2023-01-01       | 100.00 |
| 4              | 102         | 3          | 2023-01-22       | 200.00 |
| 5              | 101         | 3          | 2023-02-10       | 200.00 |
+----------------+-------------+------------+------------------+--------+
</pre>

<p>Bảng <code>Products</code>:</p>

<pre class="example-io">
+------------+----------+--------+
| product_id | category | price  |
+------------+----------+--------+
| 1          | A        | 100.00 |
| 2          | B        | 150.00 |
| 3          | C        | 200.00 |
+------------+----------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+--------------+-------------------+-------------------+------------------------+--------------+---------------+
| customer_id | total_amount | transaction_count | unique_categories | avg_transaction_amount | top_category | loyalty_score |
+-------------+--------------+-------------------+-------------------+------------------------+--------------+---------------+
| 101         | 450.00       | 3                 | 3                 | 150.00                 | C            | 34.50         |
| 102         | 300.00       | 2                 | 2                 | 150.00                 | C            | 23.00         |
+-------------+--------------+-------------------+-------------------+------------------------+--------------+---------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với khách hàng 101:
	<ul>
		<li>Tổng số tiền đã chi: 100.00 + 150.00 + 200.00 = 450.00</li>
		<li>Số lượng giao dịch: 3</li>
		<li>Các danh mục khác nhau: A, B, C (3 danh mục)</li>
		<li>Số tiền giao dịch trung bình: 450.00 / 3 = 150.00</li>
		<li>Danh mục đứng đầu: C (Khách hàng 101 đã mua mỗi danh mục A, B và C 1 lần. Vì số lần mua của cả ba danh mục bằng nhau, ta chọn giao dịch gần đây nhất, thuộc danh mục C vào ngày 2023-02-10)</li>
		<li>Điểm trung thành: (3 * 10) + (450.00 / 100) = 34.50</li>
	</ul>
	</li>
	<li>Với khách hàng 102:
	<ul>
		<li>Tổng số tiền đã chi: 100.00 + 200.00 = 300.00</li>
		<li>Số lượng giao dịch: 2</li>
		<li>Các danh mục khác nhau: A, C (2 danh mục)</li>
		<li>Số tiền giao dịch trung bình: 300.00 / 2 = 150.00</li>
		<li>Danh mục đứng đầu: C (Khách hàng 102 đã mua mỗi danh mục A và C 1 lần. Vì số lần mua của hai danh mục bằng nhau, ta chọn giao dịch gần đây nhất, thuộc danh mục C vào ngày 2023-01-22)</li>
		<li>Điểm trung thành: (2 * 10) + (300.00 / 100) = 23.00</li>
	</ul>
	</li>
</ul>

<p><strong>Lưu ý:</strong> Kết quả được sắp xếp theo loyalty_score theo thứ tự giảm dần, sau đó theo customer_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm + Hàm cửa sổ + Join

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tổng hợp số tiền, số lượng giao dịch và số danh mục khác nhau theo từng khách hàng, đồng thời tìm danh mục được mua thường xuyên nhất, rồi ưu tiên giao dịch gần đây nhất khi hòa. Nếu xử lý bằng code ứng dụng, logic sẽ bị phân tán.
>
> Hãy join giao dịch với sản phẩm, đếm theo từng khách hàng và danh mục kèm ngày gần đây nhất, lấy hạng $1$ cho mỗi khách hàng theo thứ tự đó, rồi tổng hợp điểm trung thành trên các dòng chi tiết. Hàm cửa sổ biểu diễn chính xác quy tắc phá hòa.

<!-- thinking:end -->

Trước tiên, ta join bảng `Transactions` với bảng `Products` và lưu kết quả vào bảng tạm `T`.

Tiếp theo, ta dùng bảng `T` để tính số lượng giao dịch và ngày giao dịch gần đây nhất của từng khách hàng trong mỗi danh mục, rồi lưu kết quả vào bảng tạm `P`.

Sau đó, ta dùng bảng `P` để tính thứ hạng của số lượng giao dịch của từng khách hàng trong mỗi danh mục, rồi lưu kết quả vào bảng tạm `R`.

Cuối cùng, ta dùng các bảng `T` và `R` để tính tổng số tiền giao dịch, số lượng giao dịch, số danh mục khác nhau, số tiền giao dịch trung bình, danh mục được mua thường xuyên nhất và điểm trung thành của từng khách hàng, sau đó trả về kết quả theo thứ tự điểm trung thành giảm dần và ID khách hàng tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT *
        FROM
            Transactions
            JOIN Products USING (product_id)
    ),
    P AS (
        SELECT
            customer_id,
            category,
            COUNT(1) cnt,
            MAX(transaction_date) max_date
        FROM T
        GROUP BY 1, 2
    ),
    R AS (
        SELECT
            customer_id,
            category,
            RANK() OVER (
                PARTITION BY customer_id
                ORDER BY cnt DESC, max_date DESC
            ) rk
        FROM P
    )
SELECT
    t.customer_id,
    ROUND(SUM(amount), 2) total_amount,
    COUNT(1) transaction_count,
    COUNT(DISTINCT t.category) unique_categories,
    ROUND(AVG(amount), 2) avg_transaction_amount,
    r.category top_category,
    ROUND(COUNT(1) * 10 + SUM(amount) / 100, 2) loyalty_score
FROM
    T t
    JOIN R r ON t.customer_id = r.customer_id AND r.rk = 1
GROUP BY 1
ORDER BY 7 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
