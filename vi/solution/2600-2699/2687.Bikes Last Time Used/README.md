---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2687. Bikes Last Time Used 🔒](https://leetcode.com/problems/bikes-last-time-used)

[中文文档](/solution/2600-2699/2687.Bikes%20Last%20Time%20Used/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Bikes</font></code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| ride_id     | int      |
| bike_number | int      |
| start_time  | datetime |
| end_time    | datetime |
+-------------+----------+
Cột ride_id chứa các giá trị duy nhất.
Mỗi hàng chứa thông tin về một chuyến đi, bao gồm ride_id, số xe, thời gian bắt đầu và kết thúc chuyến đi.
Đảm bảo rằng start_time và end_time là các giá trị datetime hợp lệ.

</pre>

<p>Hãy viết lời giải để tìm <strong>thời điểm</strong> <strong>cuối cùng</strong> mỗi xe được sử dụng.</p>

<p>Trả về bảng kết quả được sắp xếp theo những xe được <strong>sử dụng gần đây nhất</strong>.&nbsp;</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong><code>Bikes</code> bảng:
+---------+-------------+---------------------+---------------------+
| ride_id | bike_number | start_time          | end_time            |
+---------+-------------+---------------------+---------------------+
| 1       | W00576      | 2012-03-25 11:30:00 | 2012-03-25 12:40:00 |
| 2       | W00300      | 2012-03-25 10:30:00 | 2012-03-25 10:50:00 |
| 3       | W00455      | 2012-03-26 14:30:00 | 2012-03-26 17:40:00 |
| 4       | W00455      | 2012-03-25 12:30:00 | 2012-03-25 13:40:00 |
| 5       | W00576      | 2012-03-25 08:10:00 | 2012-03-25 09:10:00 |
| 6       | W00576      | 2012-03-28 02:30:00 | 2012-03-28 02:50:00 |
+---------+-------------+---------------------+---------------------+

<strong>Đầu ra:</strong>
+-------------+---------------------+
| bike_number | end_time            |
+-------------+---------------------+
| W00576      | 2012-03-28 02:50:00 |
| W00455      | 2012-03-26 17:40:00 |
| W00300      | 2012-03-25 10:50:00 |
+-------------+---------------------+
<strong>Giải thích:</strong>
xe có số W00576 có ba chuyến đi, trong đó chuyến gần nhất là chuyến có ride_id 6, kết thúc vào lúc 2012-03-28 02:50:00.
xe có số W00300 chỉ có 1 chuyến đi, nên ta đưa trực tiếp end_time vào kết quả.
xe có số W00455 có hai chuyến đi, trong đó chuyến gần nhất là chuyến có ride_id 3, kết thúc vào lúc 2012-03-26 17:40:00.
Trả về kết quả theo thứ tự những xe được sử dụng gần đây nhất.
</pre>

<p>&nbsp;</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một xe có thể được thuê nhiều lần; chỉ cần thời điểm kết thúc gần nhất. Dùng `GROUP BY bike_number` với `MAX(end_time)`, sau đó sắp xếp theo thời điểm đó theo thứ tự giảm dần.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    bike_number,
    MAX(end_time) AS end_time
FROM Bikes
GROUP BY bike_number
ORDER BY end_time DESC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
