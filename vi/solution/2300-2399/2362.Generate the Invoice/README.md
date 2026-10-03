---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2362. Generate the Invoice 🔒](https://leetcode.com/problems/generate-the-invoice)

[中文文档](/solution/2300-2399/2362.Generate%20the%20Invoice/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Products</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| product_id  | int  |
| price       | int  |
+-------------+------+
product_id chứa các giá trị không trùng lặp.
Mỗi hàng trong bảng này cho biết ID của một sản phẩm và giá của một đơn vị sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Purchases</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| invoice_id  | int  |
| product_id  | int  |
| quantity    | int  |
+-------------+------+
(invoice_id, product_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này cho biết số lượng được đặt của một sản phẩm trong một hóa đơn.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để hiển thị thông tin chi tiết của hóa đơn có giá cao nhất. Nếu có từ hai hóa đơn trở lên có cùng giá, hãy trả về thông tin chi tiết của hóa đơn có <code>invoice_id</code> nhỏ nhất.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Products:
+------------+-------+
| product_id | price |
+------------+-------+
| 1          | 100   |
| 2          | 200   |
+------------+-------+
Bảng Purchases:
+------------+------------+----------+
| invoice_id | product_id | quantity |
+------------+------------+----------+
| 1          | 1          | 2        |
| 3          | 2          | 1        |
| 2          | 2          | 3        |
| 2          | 1          | 4        |
| 4          | 1          | 10       |
+------------+------------+----------+
<strong>Đầu ra:</strong>
+------------+----------+-------+
| product_id | quantity | price |
+------------+----------+-------+
| 2          | 3        | 600   |
| 1          | 4        | 400   |
+------------+----------+-------+
<strong>Giải thích:</strong>
Hóa đơn 1: giá = (2 * 100) = $200
Hóa đơn 2: giá = (4 * 100) + (3 * 200) = $1000
Hóa đơn 3: giá = (1 * 200) = $200
Hóa đơn 4: giá = (10 * 100) = $1000

Giá cao nhất là $1000, và các hóa đơn có giá cao nhất là 2 và 4. Ta trả về thông tin chi tiết của hóa đơn có ID nhỏ nhất, tức là hóa đơn 2.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần các dòng chi tiết của hóa đơn có tổng tiền lớn nhất, với trường hợp bằng nhau thì chọn ID nhỏ nhất. Trước hết, hãy tổng hợp số tiền, sau đó join ngược lại với các dòng chi tiết.
>
> Join purchases với products, tính tổng theo $invoice\_id$, lấy dòng đầu tiên theo số tiền giảm dần rồi ID tăng dần, sau đó join thêm lần nữa để lấy số lượng và giá của từng sản phẩm.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT *
        FROM
            Purchases
            JOIN Products USING (product_id)
    ),
    T AS (
        SELECT invoice_id, SUM(price * quantity) AS amount
        FROM P
        GROUP BY invoice_id
        ORDER BY 2 DESC, 1
        LIMIT 1
    )
SELECT product_id, quantity, (quantity * price) AS price
FROM
    P
    JOIN T USING (invoice_id);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
