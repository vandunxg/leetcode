---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1809. Ad-Free Sessions 🔒](https://leetcode.com/problems/ad-free-sessions)

[中文文档](/solution/1800-1899/1809.Ad-Free%20Sessions/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Playback</code></p>

<pre>
+-------------+------+
| Tên cột     | Kiểu |
+-------------+------+
| session_id  | int  |
| customer_id | int  |
| start_time  | int  |
| end_time    | int  |
+-------------+------+
session_id là cột chứa các giá trị duy nhất trong bảng này.
customer_id là ID của khách hàng xem session này.
Session diễn ra trong khoảng [start_time, end_time] <strong>bao gồm cả hai đầu mút</strong>.
Đảm bảo rằng start_time &lt;= end_time và hai session của cùng một khách hàng không giao nhau.</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Ads</code></p>

<pre>
+-------------+------+
| Tên cột     | Kiểu |
+-------------+------+
| ad_id       | int  |
| customer_id | int  |
| timestamp   | int  |
+-------------+------+
ad_id là cột chứa các giá trị duy nhất trong bảng này.
customer_id là ID của khách hàng xem quảng cáo này.
timestamp là thời điểm quảng cáo được hiển thị.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo tất cả session không hiển thị bất kỳ quảng cáo nào.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả giống với ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Playback:
+------------+-------------+------------+----------+
| session_id | customer_id | start_time | end_time |
+------------+-------------+------------+----------+
| 1          | 1           | 1          | 5        |
| 2          | 1           | 15         | 23       |
| 3          | 2           | 10         | 12       |
| 4          | 2           | 17         | 28       |
| 5          | 2           | 2          | 8        |
+------------+-------------+------------+----------+
Bảng Ads:
+-------+-------------+-----------+
| ad_id | customer_id | timestamp |
+-------+-------------+-----------+
| 1     | 1           | 5         |
| 2     | 2           | 17        |
| 3     | 2           | 20        |
+-------+-------------+-----------+
<strong>Đầu ra:</strong>
+------------+
| session_id |
+------------+
| 2          |
| 3          |
| 5          |
+------------+
<strong>Giải thích:</strong>
Quảng cáo có ID 1 được hiển thị cho người dùng 1 tại thời điểm 5, khi họ đang ở session 1.
Quảng cáo có ID 2 được hiển thị cho người dùng 2 tại thời điểm 17, khi họ đang ở session 4.
Quảng cáo có ID 3 được hiển thị cho người dùng 2 tại thời điểm 20, khi họ đang ở session 4.
Ta thấy session 1 và 4 có ít nhất một quảng cáo. Session 2, 3 và 5 không có quảng cáo nào, nên ta trả về chúng.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm các session không bao giờ giao với quảng cáo của cùng một khách hàng. Nếu dùng code ứng dụng để kiểm tra mọi quảng cáo cho từng session, chi phí sẽ lớn khi bảng có nhiều dữ liệu.
>
> Một subquery join Playback với Ads theo cùng $customer\_id$, đồng thời timestamp của quảng cáo nằm trong $[start\_time,end\_time]$. Query bên ngoài giữ lại các giá trị $session\_id$ không xuất hiện trong tập đó, nhờ vậy mọi session bị gián đoạn đều bị loại trong một lần truy vấn.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT session_id
FROM Playback
WHERE
    session_id NOT IN (
        SELECT session_id
        FROM
            Playback AS p
            JOIN Ads AS a
                ON p.customer_id = a.customer_id AND a.timestamp BETWEEN p.start_time AND p.end_time
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
