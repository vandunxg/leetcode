---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2686. Immediate Food Delivery III 🔒](https://leetcode.com/problems/immediate-food-delivery-iii)

[中文文档](/solution/2600-2699/2686.Immediate%20Food%20Delivery%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Delivery</code></p>

<pre>
+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+
delivery_id là cột có các giá trị duy nhất của bảng này.
Mỗi hàng chứa thông tin về việc giao đồ ăn cho một khách hàng đặt hàng vào một ngày nào đó và chỉ định ngày giao mong muốn (vào ngày đặt hàng hoặc sau đó).
</pre>

<p>Nếu ngày giao mong muốn của khách hàng trùng với ngày đặt hàng, đơn hàng được gọi là <strong>giao ngay</strong>, nếu không thì là <strong>đặt lịch</strong>.</p>

<p>Hãy viết lời giải để tìm tỷ lệ phần trăm đơn hàng giao ngay trên mỗi <code>order_date</code> khác nhau, <strong>làm tròn đến 2 chữ số thập phân</strong>.&nbsp;</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>order_date</code> <em>theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Delivery:
+-------------+-------------+------------+-----------------------------+
| delivery_id | customer_id | order_date | customer_pref_delivery_date |
+-------------+-------------+------------+-----------------------------+
| 1           | 1           | 2019-08-01 | 2019-08-02                  |
| 2           | 2           | 2019-08-01 | 2019-08-01                  |
| 3           | 1           | 2019-08-01 | 2019-08-01                  |
| 4           | 3           | 2019-08-02 | 2019-08-13                  |
| 5           | 3           | 2019-08-02 | 2019-08-02                  |
| 6           | 2           | 2019-08-02 | 2019-08-02                  |
| 7           | 4           | 2019-08-03 | 2019-08-03                  |
| 8           | 1           | 2019-08-03 | 2019-08-03                  |
| 9           | 5           | 2019-08-04 | 2019-08-08                  |
| 10          | 2           | 2019-08-04 | 2019-08-18                  |
+-------------+-------------+------------+-----------------------------+
<strong>Đầu ra:</strong>
+------------+----------------------+
| order_date | immediate_percentage |
+------------+----------------------+
| 2019-08-01 | 66.67                |
| 2019-08-02 | 66.67                |
| 2019-08-03 | 100.00               |
| 2019-08-04 | 0.00                 |
+------------+----------------------+
<strong>Giải thích:</strong>
- Vào ngày 2019-08-01 có ba đơn hàng, trong đó hai đơn là giao ngay và một đơn là đặt lịch. Vì vậy, tỷ lệ đơn hàng giao ngay trong ngày đó là 66.67.
- Vào ngày 2019-08-02 có ba đơn hàng, trong đó hai đơn là giao ngay và một đơn là đặt lịch. Vì vậy, tỷ lệ đơn hàng giao ngay trong ngày đó là 66.67.
- Vào ngày 2019-08-03 có hai đơn hàng và cả hai đều được giao ngay. Vì vậy, tỷ lệ đơn hàng giao ngay trong ngày đó là 100.00.
- Vào ngày 2019-08-04 có hai đơn hàng và cả hai đều được đặt lịch. Vì vậy, tỷ lệ đơn hàng giao ngay trong ngày đó là 0.00.
order_date được sắp xếp theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần tỷ lệ đơn hàng giao ngay theo ngày, tức là các hàng có `customer_pref_delivery_date = order_date`. Nhóm theo `order_date`, chia số lượng thỏa điều kiện cho kích thước nhóm, làm tròn đến hai chữ số thập phân rồi sắp xếp theo ngày.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    order_date,
    ROUND(
        100 * SUM(IF(customer_pref_delivery_date = order_date, 1, 0)) / COUNT(*),
        2
    ) AS immediate_percentage
FROM Delivery
GROUP BY order_date
ORDER BY order_date;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
