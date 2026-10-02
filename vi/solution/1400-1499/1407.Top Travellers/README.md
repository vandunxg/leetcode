---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1407. Top Travellers](https://leetcode.com/problems/top-travellers)

[中文文档](/solution/1400-1499/1407.Top%20Travellers/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Users</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
id là cột chứa các giá trị duy nhất trong bảng này.
name là tên của người dùng.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Rides</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| user_id       | int     |
| distance      | int     |
+---------------+---------+
id là cột chứa các giá trị duy nhất trong bảng này.
user_id là id của người dùng đã di chuyển quãng đường &quot;distance&quot;.
</pre>

<p>&nbsp;</p>

<p>Hãy viết một lời giải để báo cáo quãng đường đã di chuyển của mỗi người dùng.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>travelled_distance</code> theo <strong>thứ tự giảm dần</strong>; nếu có từ hai người dùng trở lên di chuyển cùng một quãng đường, hãy sắp xếp họ theo <code>name</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng của bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Users:
+------+-----------+
| id   | name      |
+------+-----------+
| 1    | Alice     |
| 2    | Bob       |
| 3    | Alex      |
| 4    | Donald    |
| 7    | Lee       |
| 13   | Jonathan  |
| 19   | Elvis     |
+------+-----------+
Bảng Rides:
+------+----------+----------+
| id   | user_id  | distance |
+------+----------+----------+
| 1    | 1        | 120      |
| 2    | 2        | 317      |
| 3    | 3        | 222      |
| 4    | 7        | 100      |
| 5    | 13       | 312      |
| 6    | 19       | 50       |
| 7    | 7        | 120      |
| 8    | 19       | 400      |
| 9    | 7        | 230      |
+------+----------+----------+
<strong>Đầu ra:</strong> 
+----------+--------------------+
| name     | travelled_distance |
+----------+--------------------+
| Elvis    | 450                |
| Lee      | 450                |
| Bob      | 317                |
| Jonathan | 312                |
| Alex     | 222                |
| Alice    | 120                |
| Donald   | 0                  |
+----------+--------------------+
<strong>Giải thích:</strong> 
Elvis và Lee đều đã di chuyển 450 dặm; Elvis đứng đầu vì tên của anh ấy đứng trước Lee theo thứ tự alphabet.
Bob, Jonathan, Alex và Alice chỉ có một chuyến đi, nên chúng ta chỉ cần sắp xếp họ theo tổng quãng đường đã di chuyển.
Donald không có chuyến đi nào, nên quãng đường anh ấy đã di chuyển là 0.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: LEFT JOIN + GROUP BY

<!-- thinking:start -->

> **Tư duy**
>
> Mọi người dùng đều phải xuất hiện, bao gồm cả những người không có chuyến đi. Inner join sẽ loại bỏ họ, nên `Users` là bảng bên trái được join với `Rides`.
>
> Group theo `id` của người dùng và tính tổng `distance`. `SUM` là `NULL` khi không có chuyến đi, nên `IFNULL` chuyển nó thành $0$. Sắp xếp theo quãng đường giảm dần, sau đó theo tên.

<!-- thinking:end -->

Ta có thể dùng left join để join bảng `Users` với bảng `Rides` theo điều kiện id của người dùng, sau đó group theo id người dùng để tính quãng đường đã di chuyển của mỗi người. Lưu ý rằng nếu người dùng không có bản ghi di chuyển nào thì quãng đường đã di chuyển là $0$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT name, IFNULL(SUM(distance), 0) AS travelled_distance
FROM
    Users AS u
    LEFT JOIN Rides AS r ON u.id = r.user_id
GROUP BY u.id
ORDER BY 2 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
