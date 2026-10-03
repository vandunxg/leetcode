---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2238. Number of Times a Driver Was a Passenger 🔒](https://leetcode.com/problems/number-of-times-a-driver-was-a-passenger)

[中文文档](/solution/2200-2299/2238.Number%20of%20Times%20a%20Driver%20Was%20a%20Passenger/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Rides</code></p>

<pre>
+--------------+------+
| Tên cột      | Kiểu |
+--------------+------+
| ride_id      | int  |
| driver_id    | int  |
| passenger_id | int  |
+--------------+------+
ride_id là cột chứa các giá trị không trùng nhau.
Mỗi hàng của bảng này chứa ID của tài xế và ID của hành khách đã đi trong chuyến đi có ride_id.
Lưu ý rằng driver_id != passenger_id.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo ID của mỗi tài xế và số lần họ làm hành khách.</p>

<p>Trả về bảng kết quả theo thứ tự <strong>bất kỳ</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Rides:
+---------+-----------+--------------+
| ride_id | driver_id | passenger_id |
+---------+-----------+--------------+
| 1       | 7         | 1            |
| 2       | 7         | 2            |
| 3       | 11        | 1            |
| 4       | 11        | 7            |
| 5       | 11        | 7            |
| 6       | 11        | 3            |
+---------+-----------+--------------+
<strong>Đầu ra:</strong>
+-----------+-----+
| driver_id | cnt |
+-----------+-----+
| 7         | 2   |
| 11        | 0   |
+-----------+-----+
<strong>Giải thích:</strong>
Có hai tài xế trong tất cả các chuyến đi đã cho: 7 và 11.
Tài xế có ID = 7 đã làm hành khách hai lần.
Tài xế có ID = 11 chưa bao giờ làm hành khách.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi tài xế xuất hiện, ta đếm số lần người đó đi với vai trò hành khách; nếu chưa từng đi thì số đếm là $0$. Nếu nhóm $\textit{Rides}$ theo hành khách, những người chỉ lái xe sẽ bị bỏ sót.
>
> Lấy các giá trị $\textit{driver\_id}$ không trùng nhau rồi left join với $\textit{Rides}$ theo điều kiện $\textit{driver\_id} = \textit{passenger\_id}$. Đếm $\textit{passenger\_id}$ để giữ lại các giá trị bằng 0.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH T AS (SELECT DISTINCT driver_id FROM Rides)
SELECT t.driver_id, COUNT(passenger_id) AS cnt
FROM
    T AS t
    LEFT JOIN Rides AS r ON t.driver_id = r.passenger_id
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
