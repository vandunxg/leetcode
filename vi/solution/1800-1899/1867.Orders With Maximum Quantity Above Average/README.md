---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1867. Orders With Maximum Quantity Above Average 🔒](https://leetcode.com/problems/orders-with-maximum-quantity-above-average)

[中文文档](/solution/1800-1899/1867.Orders%20With%20Maximum%20Quantity%20Above%20Average/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>OrdersDetails</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| product_id  | int  |
| quantity    | int  |
+-------------+------+
(order_id, product_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Một đơn hàng được biểu diễn bằng nhiều hàng, mỗi hàng tương ứng với một sản phẩm trong đơn hàng.
Mỗi hàng của bảng này chứa số lượng sản phẩm product_id được đặt trong đơn hàng order_id.
</pre>

<p>&nbsp;</p>

<p>Bạn đang vận hành một trang thương mại điện tử và muốn tìm các <strong>đơn hàng mất cân đối</strong>. Một <strong>đơn hàng mất cân đối</strong> là đơn hàng có số lượng <strong>lớn nhất</strong> <strong>lớn hơn nghiêm ngặt</strong> số lượng <strong>trung bình </strong>của <strong>mọi đơn hàng (bao gồm chính nó)</strong>.</p>

<p>Số lượng <strong>trung bình </strong>của một đơn hàng được tính bằng <code>(total quantity of all products in the order) / (number of different products in the order)</code>. Số lượng <strong>lớn nhất</strong> của một đơn hàng là <code>quantity</code> cao nhất của một sản phẩm bất kỳ trong đơn hàng.</p>

<p>Hãy viết lời giải để tìm <code>order_id</code> của tất cả <strong>đơn hàng mất cân đối</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng OrdersDetails:
+----------+------------+----------+
| order_id | product_id | quantity |
+----------+------------+----------+
| 1        | 1          | 12       |
| 1        | 2          | 10       |
| 1        | 3          | 15       |
| 2        | 1          | 8        |
| 2        | 4          | 4        |
| 2        | 5          | 6        |
| 3        | 3          | 5        |
| 3        | 4          | 18       |
| 4        | 5          | 2        |
| 4        | 6          | 8        |
| 5        | 7          | 9        |
| 5        | 8          | 9        |
| 3        | 9          | 20       |
| 2        | 9          | 4        |
+----------+------------+----------+
<strong>Đầu ra:</strong>
+----------+
| order_id |
+----------+
| 1        |
| 3        |
+----------+
<strong>Giải thích:</strong>
Số lượng trung bình của mỗi đơn hàng là:
- order_id=1: (12+10+15)/3 = 12.3333333
- order_id=2: (8+4+6+4)/4 = 5.5
- order_id=3: (5+18+20)/3 = 14.333333
- order_id=4: (2+8)/2 = 5
- order_id=5: (9+9)/2 = 9

Số lượng lớn nhất của mỗi đơn hàng là:
- order_id=1: max(12, 10, 15) = 15
- order_id=2: max(8, 4, 6, 4) = 8
- order_id=3: max(5, 18, 20) = 20
- order_id=4: max(2, 8) = 8
- order_id=5: max(9, 9) = 9

Đơn hàng 1 và 3 là các đơn hàng mất cân đối vì chúng có số lượng lớn nhất vượt quá số lượng trung bình của mọi đơn hàng.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần những đơn hàng có quantity lớn nhất theo từng dòng lớn hơn nghiêm ngặt giá trị lớn nhất trong các mức trung bình của từng đơn hàng. Cả giá trị lớn nhất và giá trị trung bình đều là các phép tổng hợp theo $order\_id$.
>
> Tính $MAX(quantity)$ và $SUM/COUNT$ cho từng đơn hàng, lấy $MAX(avg\_quantity)$ trong một subquery làm ngưỡng, rồi giữ lại các đơn hàng có quantity lớn nhất vượt ngưỡng đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            order_id,
            MAX(quantity) AS max_quantity,
            SUM(quantity) / COUNT(1) AS avg_quantity
        FROM OrdersDetails
        GROUP BY order_id
    )
SELECT order_id
FROM t
WHERE max_quantity > (SELECT MAX(avg_quantity) FROM t);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
