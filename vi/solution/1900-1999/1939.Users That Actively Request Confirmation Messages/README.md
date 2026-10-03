---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1939. Users That Actively Request Confirmation Messages 🔒](https://leetcode.com/problems/users-that-actively-request-confirmation-messages)

[中文文档](/solution/1900-1999/1939.Users%20That%20Actively%20Request%20Confirmation%20Messages/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Signups</code></p>

<pre>
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
user_id là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng chứa thông tin về thời điểm đăng ký của người dùng có ID user_id.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Confirmations</code></p>

<pre>
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+
(user_id, time_stamp) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
user_id là khóa ngoại (cột tham chiếu) đến bảng Signups.
action là ENUM (nhóm) thuộc kiểu (&#39;confirmed&#39;, &#39;timeout&#39;).
Mỗi hàng của bảng này cho biết người dùng có ID user_id đã yêu cầu một tin nhắn xác nhận tại time_stamp và tin nhắn đó hoặc đã được xác nhận (&#39;confirmed&#39;) hoặc đã hết hạn mà không được xác nhận (&#39;timeout&#39;).</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm ID của những người dùng đã yêu cầu tin nhắn xác nhận <strong>hai lần</strong> trong khoảng thời gian 24 giờ. Hai tin nhắn cách nhau đúng 24 giờ được xem là nằm trong khoảng thời gian này. <code>action</code> không ảnh hưởng đến đáp án, chỉ thời điểm yêu cầu mới được xét.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Signups:
+---------+---------------------+
| user_id | time_stamp          |
+---------+---------------------+
| 3       | 2020-03-21 10:16:13 |
| 7       | 2020-01-04 13:57:59 |
| 2       | 2020-07-29 23:09:44 |
| 6       | 2020-12-09 10:39:37 |
+---------+---------------------+
Bảng Confirmations:
+---------+---------------------+-----------+
| user_id | time_stamp          | action    |
+---------+---------------------+-----------+
| 3       | 2021-01-06 03:30:46 | timeout   |
| 3       | 2021-01-06 03:37:45 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 11:57:30 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-01-23 00:00:00 | timeout   |
| 6       | 2021-10-23 14:14:14 | confirmed |
| 6       | 2021-10-24 14:14:13 | timeout   |
+---------+---------------------+-----------+
<strong>Đầu ra:</strong>
+---------+
| user_id |
+---------+
| 2       |
| 3       |
| 6       |
+---------+
<strong>Giải thích:</strong>
Người dùng 2 đã yêu cầu hai tin nhắn cách nhau đúng 24 giờ, nên được đưa vào kết quả.
Người dùng 3 đã yêu cầu hai tin nhắn cách nhau 6 phút 59 giây, nên được đưa vào kết quả.
Người dùng 6 đã yêu cầu hai tin nhắn cách nhau 23 giờ 59 phút 59 giây, nên được đưa vào kết quả.
Người dùng 7 đã yêu cầu hai tin nhắn cách nhau 24 giờ 1 giây, nên bị loại khỏi kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một người dùng được xem là active nếu hai timestamp xác nhận nằm trong khoảng $24$ giờ. Phép self-join biểu diễn trực tiếp điều kiện này.
>
> Nối $\texttt{Confirmations}$ với chính nó theo $\texttt{user\_id}$, trong đó $c_1$ sớm hơn $c_2$ và chênh lệch tính bằng giây không quá một ngày, sau đó lấy các người dùng khác nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT DISTINCT user_id
FROM
    Confirmations AS c1
    JOIN Confirmations AS c2 USING (user_id)
WHERE
    c1.time_stamp < c2.time_stamp
    AND TIMESTAMPDIFF(SECOND, c1.time_stamp, c2.time_stamp) <= 24 * 60 * 60;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
