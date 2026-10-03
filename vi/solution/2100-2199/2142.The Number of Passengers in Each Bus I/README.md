---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2142. The Number of Passengers in Each Bus I 🔒](https://leetcode.com/problems/the-number-of-passengers-in-each-bus-i)

[中文文档](/solution/2100-2199/2142.The%20Number%20of%20Passengers%20in%20Each%20Bus%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Buses</code></p>

<pre>
+--------------+------+
| Tên cột      | Kiểu |
+--------------+------+
| bus_id       | int  |
| arrival_time | int  |
+--------------+------+
bus_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa thông tin về thời gian một xe buýt đến trạm LeetCode.
Không có hai xe buýt nào đến cùng một thời điểm.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Passengers</code></p>

<pre>
+--------------+------+
| Tên cột      | Kiểu |
+--------------+------+
| passenger_id | int  |
| arrival_time | int  |
+--------------+------+
passenger_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa thông tin về thời gian một hành khách đến trạm LeetCode.
</pre>

<p>&nbsp;</p>

<p>Xe buýt và hành khách đến trạm LeetCode. Nếu một xe buýt đến trạm tại thời điểm <code>t<sub>bus</sub></code> và một hành khách đến trạm tại thời điểm <code>t<sub>passenger</sub></code>, trong đó <code>t<sub>passenger</sub> &lt;= t<sub>bus</sub></code> và hành khách chưa lên bất kỳ xe buýt nào, hành khách đó sẽ đi xe buýt này.</p>

<p>Hãy viết một lời giải để báo cáo số hành khách đã đi mỗi xe buýt.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>bus_id</code> theo thứ tự <strong>tăng dần</strong>.</p>

<p>Định dạng của bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Buses:
+--------+--------------+
| bus_id | arrival_time |
+--------+--------------+
| 1      | 2            |
| 2      | 4            |
| 3      | 7            |
+--------+--------------+
Bảng Passengers:
+--------------+--------------+
| passenger_id | arrival_time |
+--------------+--------------+
| 11           | 1            |
| 12           | 5            |
| 13           | 6            |
| 14           | 7            |
+--------------+--------------+
<strong>Đầu ra:</strong>
+--------+----------------+
| bus_id | passengers_cnt |
+--------+----------------+
| 1      | 1              |
| 2      | 0              |
| 3      | 3              |
+--------+----------------+
<strong>Giải thích:</strong>
- Hành khách 11 đến vào thời điểm 1.
- Xe buýt 1 đến vào thời điểm 2 và đón hành khách 11.

- Xe buýt 2 đến vào thời điểm 4 và không đón hành khách nào.

- Hành khách 12 đến vào thời điểm 5.
- Hành khách 13 đến vào thời điểm 6.
- Hành khách 14 đến vào thời điểm 7.
- Xe buýt 3 đến vào thời điểm 7 và đón các hành khách 12, 13 và 14.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi xe buýt đón những hành khách đến không muộn hơn xe buýt và chưa lên xe. Việc duyệt qua mọi hành khách cho từng xe buýt khá bất tiện trong SQL và khiến công việc bị lặp lại.
>
> Nối mỗi hành khách với mọi xe buýt không khởi hành trước thời điểm hành khách đến; hiệu giữa các tổng lũy kế theo thời gian đến của xe buýt chính là số hành khách mới.
>
> Sau phép nối trái và phép đếm theo từng xe buýt, $\texttt{LAG}$ lấy tổng lũy kế trước đó để thực hiện phép trừ, với thứ tự theo $\textit{bus\_id}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    bus_id,
    COUNT(passenger_id) - LAG(COUNT(passenger_id), 1, 0) OVER (
        ORDER BY b.arrival_time
    ) AS passengers_cnt
FROM
    Buses AS b
    LEFT JOIN Passengers AS p ON p.arrival_time <= b.arrival_time
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
