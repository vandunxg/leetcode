---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2153. The Number of Passengers in Each Bus II 🔒](https://leetcode.com/problems/the-number-of-passengers-in-each-bus-ii)

[中文文档](/solution/2100-2199/2153.The%20Number%20of%20Passengers%20in%20Each%20Bus%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Buses</code></p>

<pre>
+--------------+------+
| Tên cột      | Kiểu |
+--------------+------+
| bus_id       | int  |
| arrival_time | int  |
| capacity     | int  |
+--------------+------+
bus_id chứa các giá trị duy nhất.
Mỗi hàng của bảng này chứa thông tin về thời gian một xe buýt đến trạm LeetCode và sức chứa của xe (số ghế trống mà xe có).
Không có hai xe buýt nào đến cùng một thời điểm và sức chứa của tất cả xe buýt đều là số nguyên dương.
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
passenger_id chứa các giá trị duy nhất.
Mỗi hàng của bảng này chứa thông tin về thời gian một hành khách đến trạm LeetCode.
</pre>

<p>&nbsp;</p>

<p>Các xe buýt và hành khách đến trạm LeetCode. Nếu một xe buýt đến trạm tại thời điểm <code>t<sub>bus</sub></code> và một hành khách đến vào thời điểm <code>t<sub>passenger</sub></code>, trong đó <code>t<sub>passenger</sub> &lt;= t<sub>bus</sub></code> và hành khách đó chưa lên xe buýt nào, thì hành khách sẽ đi xe buýt này. Ngoài ra, mỗi xe buýt có một sức chứa. Nếu tại thời điểm xe buýt đến trạm có nhiều hành khách đang chờ hơn sức chứa <code>capacity</code> của xe, thì chỉ có <code>capacity</code> hành khách được lên xe.</p>

<p>Hãy viết một lời giải để báo cáo số hành khách đã đi mỗi xe buýt.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>bus_id</code> theo thứ tự <strong>tăng dần</strong>.</p>

<p>Định dạng của bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Buses:
+--------+--------------+----------+
| bus_id | arrival_time | capacity |
+--------+--------------+----------+
| 1      | 2            | 1        |
| 2      | 4            | 10       |
| 3      | 7            | 2        |
+--------+--------------+----------+
Bảng Passengers:
+--------------+--------------+
| passenger_id | arrival_time |
+--------------+--------------+
| 11           | 1            |
| 12           | 1            |
| 13           | 5            |
| 14           | 6            |
| 15           | 7            |
+--------------+--------------+
<strong>Đầu ra:</strong>
+--------+----------------+
| bus_id | passengers_cnt |
+--------+----------------+
| 1      | 1              |
| 2      | 1              |
| 3      | 2              |
+--------+----------------+
<strong>Giải thích:</strong>
- Hành khách 11 đến vào thời điểm 1.
- Hành khách 12 đến vào thời điểm 1.
- Xe buýt 1 đến vào thời điểm 2 và đón hành khách 11 vì xe có một ghế trống.

- Xe buýt 2 đến vào thời điểm 4 và đón hành khách 12 vì xe có mười ghế trống.

- Hành khách 12 đến vào thời điểm 5.
- Hành khách 13 đến vào thời điểm 6.
- Hành khách 14 đến vào thời điểm 7.
- Xe buýt 3 đến vào thời điểm 7 và đón các hành khách 12 và 13 vì xe có hai ghế trống.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xe buýt có sức chứa; hành khách lên xe theo thứ tự đến và những người chưa lên xe sẽ tiếp tục chờ. Ta cần trộn các sự kiện xe buýt và hành khách theo thời gian, đồng thời duy trì số hành khách đang chờ.
>
> Xem một xe buýt là một sức chứa dương và một hành khách là $-1$, sắp xếp theo thời gian (sau đó theo $\textit{bus\_id}$), rồi duy trì $\textit{cur\_sum}$ lũy kế để tiếp nhận hành khách và giảm đi khi một xe buýt đến.
>
> Với các hàng có $\textit{bus\_id}>0$, số hành khách lên xe bằng sức chứa trừ đi số hành khách còn lại sau khi xe buýt đó đến (hoặc bằng toàn bộ sức chứa nếu xe được lấp đầy).

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            SUM(cnt) OVER (ORDER BY dt, bus_id) AS cur,
            IF(@t > 0, @t := cnt, @t := @t + cnt) AS cur_sum
        FROM
            (
                SELECT bus_id, arrival_time AS dt, capacity AS cnt FROM Buses
                UNION ALL
                SELECT -1, arrival_time AS dt, -1 FROM Passengers
            ) AS a JOIN (SELECT @t := 0 x) AS b
    )
SELECT
    bus_id,
    IF(cur_sum > 0, cnt - cur_sum, cnt) AS passengers_cnt
FROM T
WHERE bus_id > 0
ORDER BY bus_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
