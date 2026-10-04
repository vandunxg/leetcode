---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2893. Calculate Orders Within Each Interval 🔒](https://leetcode.com/problems/calculate-orders-within-each-interval)

[中文文档](/solution/2800-2899/2893.Calculate%20Orders%20Within%20Each%20Interval/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Orders</font></code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| minute      | int  |
| order_count | int  |
+-------------+------+
minute là khóa chính của bảng này.
Mỗi hàng của bảng này chứa phút và số đơn hàng nhận được trong phút tương ứng. Tổng số hàng sẽ là bội số của 6.
</pre>

<p>Hãy viết một truy vấn để tính <strong>tổng số</strong> <strong>đơn hàng</strong><b> </b>trong mỗi <strong>khoảng</strong>. Mỗi khoảng được xác định bởi một nhóm gồm <code>6</code> phút.</p>

<ul>
	<li>Các phút từ <code>1</code> đến <code>6</code> thuộc khoảng <code>1</code>, còn các phút từ <code>7</code> đến <code>12</code> thuộc khoảng <code>2</code>, và tiếp tục như vậy.</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo <strong>interval_no</strong> theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Orders:
+--------+-------------+
| minute | order_count |
+--------+-------------+
| 1      | 0           |
| 2      | 2           |
| 3      | 4           |
| 4      | 6           |
| 5      | 1           |
| 6      | 4           |
| 7      | 1           |
| 8      | 2           |
| 9      | 4           |
| 10     | 1           |
| 11     | 4           |
| 12     | 6           |
+--------+-------------+
<strong>Đầu ra:</strong>
+-------------+--------------+
| interval_no | total_orders |
+-------------+--------------+
| 1           | 17           |
| 2           | 18           |
+-------------+--------------+
<strong>Giải thích:</strong>
- Khoảng số 1 gồm các phút từ 1 đến 6. Tổng số đơn hàng trong sáu phút này là (0 + 2 + 4 + 6 + 1 + 4) = 17.
- Khoảng số 2 gồm các phút từ 7 đến 12. Tổng số đơn hàng trong sáu phút này là (1 + 2 + 4 + 1 + 4 + 6) = 18.
Trả về bảng được sắp xếp theo interval_no theo thứ tự tăng dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi khối sáu phút là một khoảng. Tổng chạy `ROWS 5 PRECEDING` sau khi sắp xếp theo `minute`, chỉ giữ lại khi `minute` là bội số của $6$, sẽ cho tổng tại điểm cuối bên phải của mỗi khoảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            minute,
            SUM(order_count) OVER (
                ORDER BY minute
                ROWS 5 PRECEDING
            ) AS total_orders
        FROM Orders
    )
SELECT minute / 6 AS interval_no, total_orders
FROM T
WHERE minute % 6 = 0;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cửa sổ vẫn duyệt một tiền tố. Khi các phút liên tiếp, $\lfloor(minute+5)/6\rfloor$ nhóm trực tiếp các hàng và `SUM` trong mỗi nhóm là tổng của khoảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    FLOOR((minute + 5) / 6) AS interval_no,
    SUM(order_count) AS total_orders
FROM Orders
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
