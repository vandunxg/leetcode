---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2793. Status of Flight Tickets 🔒](https://leetcode.com/problems/status-of-flight-tickets)

[中文文档](/solution/2700-2799/2793.Status%20of%20Flight%20Tickets/README.md)

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
Cột <code>flight_id</code> chứa các giá trị khác nhau.
Mỗi hàng của bảng này chứa mã chuyến bay và sức chứa.
</pre>

<p>Bảng: <code>Passengers</code></p>

<pre>
+--------------+----------+
| Column Name  | Type     |
+--------------+----------+
| passenger_id | int      |
| flight_id    | int      |
| booking_time | datetime |
+--------------+----------+
Cột passenger_id chứa các giá trị khác nhau.
Cột booking_time chứa các giá trị khác nhau.
Mỗi hàng của bảng này chứa mã hành khách, thời gian đặt vé và mã chuyến bay của họ.
</pre>

<p>Hành khách đặt vé máy bay trước. Nếu một hành khách đặt vé cho một chuyến bay và chuyến bay vẫn còn ghế trống, vé của hành khách đó sẽ được <strong>Confirmed</strong>. Tuy nhiên, hành khách sẽ nằm trong <strong>Waitlist</strong> nếu chuyến bay đã đủ sức chứa.</p>

<p>Hãy viết lời giải để xác định trạng thái hiện tại của vé máy bay cho mỗi hành khách.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>passenger_id</code> <em>theo <strong>thứ tự tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Flights table:
+-----------+----------+
| flight_id | capacity |
+-----------+----------+
| 1         | 2        |
| 2         | 2        |
| 3         | 1        |
+-----------+----------+
Passengers table:
+--------------+-----------+---------------------+
| passenger_id | flight_id | booking_time        |
+--------------+-----------+---------------------+
| 101          | 1         | 2023-07-10 16:30:00 |
| 102          | 1         | 2023-07-10 17:45:00 |
| 103          | 1         | 2023-07-10 12:00:00 |
| 104          | 2         | 2023-07-05 13:23:00 |
| 105          | 2         | 2023-07-05 09:00:00 |
| 106          | 3         | 2023-07-08 11:10:00 |
| 107          | 3         | 2023-07-08 09:10:00 |
+--------------+-----------+---------------------+
<strong>Đầu ra:</strong>
+--------------+-----------+
| passenger_id | Status    |
+--------------+-----------+
| 101          | Confirmed |
| 102          | Waitlist  |
| 103          | Confirmed |
| 104          | Confirmed |
| 105          | Confirmed |
| 106          | Waitlist  |
| 107          | Confirmed |
+--------------+-----------+
<strong>Giải thích:</strong>
- Chuyến bay 1 có sức chứa 2 hành khách. Hành khách 101 và 103 là những người đầu tiên đặt vé, nên đã giành được các ghế còn trống. Do đó, vé của họ được xác nhận. Tuy nhiên, hành khách 102 là người thứ ba đặt vé cho chuyến bay này, nghĩa là không còn ghế trống. Hành khách 102 hiện được đưa vào danh sách chờ,
- Chuyến bay 2 có sức chứa 2 hành khách và có đúng hai hành khách đã đặt vé là 104 và 105. Vì số hành khách đã đặt vé khớp với số ghế còn trống, cả hai vé đều được xác nhận.
- Chuyến bay 3 có sức chứa 1 hành khách. Hành khách 107 đặt vé sớm hơn và giành được ghế duy nhất còn trống, nên vé được xác nhận. Hành khách 106 đặt vé sau hành khách 107 nên nằm trong danh sách chờ.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ghế trên một chuyến bay được xác nhận theo thứ tự thời gian đặt vé cho đến khi đạt sức chứa; các vé còn lại sẽ nằm trong danh sách chờ. Các vé có cùng thời điểm đặt đều được xác nhận nếu vẫn còn ghế.
>
> $RANK()$ phân vùng theo $flight\_id$ và sắp xếp theo $booking\_time$ để đánh dấu một hàng là Confirmed khi thứ hạng không vượt quá $capacity$. Các thứ hạng bằng nhau vẫn nằm trong ngưỡng này, nên không cần thêm tiêu chí phá hòa.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    passenger_id,
    IF(
        (
            RANK() OVER (
                PARTITION BY flight_id
                ORDER BY booking_time
            )
        ) <= capacity,
        'Confirmed',
        'Waitlist'
    ) AS Status
FROM
    Passengers
    JOIN Flights USING (flight_id)
ORDER BY passenger_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
