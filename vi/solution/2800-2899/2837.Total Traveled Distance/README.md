---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2837. Total Traveled Distance 🔒](https://leetcode.com/problems/total-traveled-distance)

[中文文档](/solution/2800-2899/2837.Total%20Traveled%20Distance/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Users</font></code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| name        | varchar |
+-------------+---------+
<code>user_id</code> là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa id và tên người dùng.
</pre>

<p>Bảng: <code>Rides</code></p>

<pre>
+--------------+------+
| Column Name  | Type |
+--------------+------+
| ride_id      | int  |
| user_id      | int  |
| distance     | int  |
+--------------+------+
ride_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa id chuyến đi, id người dùng và quãng đường đã đi.
</pre>

<p>Hãy viết lời giải để tính <code>distance</code> đã đi của <strong>từng người dùng</strong>. Nếu có người dùng chưa hoàn thành chuyến đi nào, thì <code>distance</code> của họ được xem là <code>0</code>. Kết quả cần trả về <code>user_id</code>, <code>name</code> và tổng <code>distance</code> đã đi.</p>

<p>Hãy <em>trả về bảng kết quả được sắp xếp theo </em><code>user_id</code><em> theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Users:
+---------+---------+
| user_id | name    |
+---------+---------+
| 17      | Addison |
| 14      | Ethan   |
| 4       | Michael |
| 2       | Avery   |
| 10      | Eleanor |
+---------+---------+
Bảng Rides:
+---------+---------+----------+
| ride_id | user_id | distance |
+---------+---------+----------+
| 72      | 17      | 160      |
| 42      | 14      | 161      |
| 45      | 4       | 59       |
| 32      | 2       | 197      |
| 15      | 4       | 357      |
| 56      | 2       | 196      |
| 10      | 14      | 25       |
+---------+---------+----------+
<strong>Đầu ra:</strong>
+---------+---------+-------------------+
| user_id | name    | traveled distance |
+---------+---------+-------------------+
| 2       | Avery   | 393               |
| 4       | Michael | 416               |
| 10      | Eleanor | 0                 |
| 14      | Ethan   | 186               |
| 17      | Addison | 160               |
+---------+---------+-------------------+
<strong>Giải thích:</strong>
-  Người dùng có id 2 đã hoàn thành hai chuyến đi dài 197 và 196, nên tổng quãng đường là 393.
-  Người dùng có id 4 đã hoàn thành hai chuyến đi dài 59 và 357, nên tổng quãng đường là 416.
-  Người dùng có id 14 đã hoàn thành hai chuyến đi dài 161 và 25, nên tổng quãng đường là 186.
-  Người dùng có id 16 chỉ hoàn thành một chuyến đi dài 160.
-  Người dùng có id 10 chưa hoàn thành chuyến đi nào, nên tổng quãng đường vẫn là 0.
Trả về bảng được sắp xếp theo user_id theo thứ tự tăng dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Left Join + Group By Sum

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần tổng quãng đường của mỗi người dùng và tính những người chưa có chuyến đi là $0$. Left join đảm bảo giữ lại mọi người dùng; gom nhóm theo `user_id` rồi dùng `SUM(distance)`, kết hợp với `IFNULL`, sẽ chuyển tổng null thành 0.

<!-- thinking:end -->

Chúng ta có thể dùng left join để nối hai bảng, sau đó dùng group by và sum để tính tổng quãng đường của mỗi người dùng. Lưu ý rằng nếu người dùng chưa hoàn thành chuyến đi nào, quãng đường của họ được xem là $0$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT user_id, name, IFNULL(SUM(distance), 0) AS 'traveled distance'
FROM
    Users
    LEFT JOIN Rides USING (user_id)
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
