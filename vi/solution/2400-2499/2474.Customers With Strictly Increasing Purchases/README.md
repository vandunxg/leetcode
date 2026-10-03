---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2474. Customers With Strictly Increasing Purchases 🔒](https://leetcode.com/problems/customers-with-strictly-increasing-purchases)

[Tài liệu tiếng Trung](/solution/2400-2499/2474.Customers%20With%20Strictly%20Increasing%20Purchases/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Orders</code></p>

<pre>
+--------------+------+
| Column Name  | Type |
+--------------+------+
| order_id     | int  |
| customer_id  | int  |
| order_date   | date |
| price        | int  |
+--------------+------+
order_id là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng chứa id của một đơn hàng, id của khách hàng đã đặt hàng, ngày đặt hàng và giá của đơn hàng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo ID của những khách hàng có <strong>tổng tiền mua hàng</strong> tăng nghiêm ngặt qua từng năm.</p>

<ul>
	<li><strong>Tổng tiền mua hàng</strong> của một khách hàng trong một năm là tổng giá của các đơn hàng của họ trong năm đó. Nếu khách hàng không đặt đơn nào trong một năm, ta xem tổng tiền mua hàng là <code>0</code>.</li>
	<li>Năm đầu tiên cần xét với mỗi khách hàng là năm họ <strong>đặt đơn đầu tiên</strong>.</li>
	<li>Năm cuối cùng cần xét với mỗi khách hàng là năm họ <strong>đặt đơn cuối cùng</strong>.</li>
</ul>

<p>Trả về bảng kết quả <strong>theo bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Orders:
+----------+-------------+------------+-------+
| order_id | customer_id | order_date | price |
+----------+-------------+------------+-------+
| 1        | 1           | 2019-07-01 | 1100  |
| 2        | 1           | 2019-11-01 | 1200  |
| 3        | 1           | 2020-05-26 | 3000  |
| 4        | 1           | 2021-08-31 | 3100  |
| 5        | 1           | 2022-12-07 | 4700  |
| 6        | 2           | 2015-01-01 | 700   |
| 7        | 2           | 2017-11-07 | 1000  |
| 8        | 3           | 2017-01-01 | 900   |
| 9        | 3           | 2018-11-07 | 900   |
+----------+-------------+------------+-------+
<strong>Đầu ra:</strong>
+-------------+
| customer_id |
+-------------+
| 1           |
+-------------+
<strong>Giải thích:</strong>
Khách hàng 1: Năm đầu tiên là 2019 và năm cuối cùng là 2022
  - 2019: 1100 + 1200 = 2300
  - 2020: 3000
  - 2021: 3100
  - 2022: 4700
  Ta thấy tổng tiền mua hàng tăng nghiêm ngặt qua từng năm, nên đưa khách hàng 1 vào đáp án.

Khách hàng 2: Năm đầu tiên là 2015 và năm cuối cùng là 2017
  - 2015: 700
  - 2016: 0
  - 2017: 1000
  Không đưa khách hàng 2 vào đáp án vì tổng tiền mua hàng không tăng nghiêm ngặt. Lưu ý rằng khách hàng 2 không mua hàng trong năm 2016.

Khách hàng 3: Năm đầu tiên là 2017 và năm cuối cùng là 2018
  - 2017: 900
  - 2018: 900
  Không đưa khách hàng 3 vào đáp án vì tổng tiền mua hàng không tăng nghiêm ngặt.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Những khách hàng có tổng tiền mua hàng hằng năm tăng nghiêm ngặt. Sau khi tính tổng $price$ theo khách hàng và năm, một dãy tăng nghiêm ngặt sẽ khiến $YEAR-RANK(\textit{total})$ không đổi.
>
> Dùng cửa sổ $RANK()$ trên tổng theo năm, sau đó chỉ giữ lại những khách hàng mà hiệu này chỉ có một giá trị phân biệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    customer_id
FROM
    (
        SELECT
            customer_id,
            YEAR(order_date),
            SUM(price) AS total,
            YEAR(order_date) - RANK() OVER (
                PARTITION BY customer_id
                ORDER BY SUM(price)
            ) AS rk
        FROM Orders
        GROUP BY customer_id, YEAR(order_date)
    ) AS t
GROUP BY customer_id
HAVING COUNT(DISTINCT rk) = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
