---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2783. Flight Occupancy and Waitlist Analysis 🔒](https://leetcode.com/problems/flight-occupancy-and-waitlist-analysis)

[中文文档](/solution/2700-2799/2783.Flight%20Occupancy%20and%20Waitlist%20Analysis/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Flights</font></code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| flight_id   | int  |
| capacity    | int  |
+-------------+------+
<code>flight_id</code> là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa mã chuyến bay và sức chứa của chuyến bay.
</pre>

<p>Bảng: <code>Passengers</code></p>

<pre>
+--------------+------+
| Column Name  | Type |
+--------------+------+
| passenger_id | int  |
| flight_id    | int  |
+--------------+------+
passenger_id là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa mã hành khách và mã chuyến bay.
</pre>

<p>Hành khách đặt vé cho các chuyến bay trước. Nếu một hành khách đặt vé cho một chuyến bay và chuyến bay vẫn còn ghế trống, vé của hành khách đó sẽ được <strong>xác nhận</strong>. Tuy nhiên, hành khách sẽ nằm trong <strong>danh sách chờ</strong> nếu chuyến bay đã đủ chỗ.</p>

<p>Hãy viết lời giải để báo cáo số hành khách đã đặt vé thành công (có ghế) và số hành khách nằm trong danh sách chờ cho mỗi chuyến bay.</p>

<p>Trả về bảng kết quả được sắp xếp theo<em> </em><code>flight_id</code>&nbsp;theo <strong>thứ tự</strong><em><strong>&nbsp;tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Flights:
+-----------+----------+
| flight_id | capacity |
+-----------+----------+
| 1         | 2        |
| 2         | 2        |
| 3         | 1        |
+-----------+----------+
Bảng Passengers:
+--------------+-----------+
| passenger_id | flight_id |
+--------------+-----------+
| 101          | 1         |
| 102          | 1         |
| 103          | 1         |
| 104          | 2         |
| 105          | 2         |
| 106          | 3         |
| 107          | 3         |
+--------------+-----------+
<strong>Đầu ra:</strong>
+-----------+------------+--------------+
| flight_id | booked_cnt | waitlist_cnt |
+-----------+------------+--------------+
| 1         | 2          | 1            |
| 2         | 2          | 0            |
| 3         | 1          | 1            |
+-----------+------------+--------------+
<strong>Giải thích:</strong>
- Chuyến bay 1 có sức chứa là 2. Vì có 3 hành khách đã đặt vé nên chỉ 2 hành khách có thể có ghế. Do đó, 2 hành khách được đặt vé thành công và 1 hành khách nằm trong danh sách chờ.
- Chuyến bay 2 có sức chứa là 2. Vì có đúng 2 hành khách đặt vé nên mọi người đều có thể giữ được ghế. Kết quả là 2 hành khách đặt vé thành công và không có hành khách nào trong danh sách chờ.
- Chuyến bay 3 có sức chứa là 1. Vì có 2 hành khách đã đặt vé nên chỉ 1 hành khách có thể có ghế. Do đó, 1 hành khách được đặt vé thành công và 1 hành khách nằm trong danh sách chờ.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi chuyến bay, đếm số ghế đã xác nhận và số hành khách trong danh sách chờ. Inner join sẽ loại bỏ các chuyến bay không có hành khách, còn số lượt đặt vé bị giới hạn bởi capacity.
>
> Left-join passengers vào flights và group theo $flight\_id$: $LEAST(count, capacity)$ là số lượt đặt vé thành công, $GREATEST(count-capacity, 0)$ là số hành khách trong danh sách chờ, sau đó sắp xếp theo flight id.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    flight_id,
    LEAST(COUNT(passenger_id), capacity) AS booked_cnt,
    GREATEST(COUNT(passenger_id) - capacity, 0) AS waitlist_cnt
FROM
    Flights
    LEFT JOIN Passengers USING (flight_id)
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
