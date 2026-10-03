---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate)

[中文文档](/solution/1900-1999/1934.Confirmation%20Rate/README.md)

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
user_id là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng chứa thông tin về thời điểm đăng ký của người dùng có ID là user_id.
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
action là ENUM (danh mục) có kiểu (&#39;confirmed&#39;, &#39;timeout&#39;)
Mỗi hàng của bảng này cho biết người dùng có ID là user_id đã yêu cầu một tin nhắn xác nhận tại time_stamp, và tin nhắn đó hoặc được xác nhận (&#39;confirmed&#39;) hoặc đã hết hạn mà không được xác nhận (&#39;timeout&#39;).
</pre>

<p>&nbsp;</p>

<p><strong>tỷ lệ xác nhận</strong> của một người dùng là số tin nhắn <code>&#39;confirmed&#39;</code> chia cho tổng số tin nhắn xác nhận đã yêu cầu. Tỷ lệ xác nhận của người dùng không yêu cầu tin nhắn xác nhận nào là <code>0</code>. Làm tròn tỷ lệ xác nhận đến <strong>hai chữ số thập phân</strong>.</p>

<p>Viết lời giải để tìm <strong>tỷ lệ xác nhận</strong> của mỗi người dùng.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

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
| 3       | 2021-07-14 14:00:00 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 12:58:28 | confirmed |
| 7       | 2021-06-14 13:59:27 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-02-28 23:59:59 | timeout   |
+---------+---------------------+-----------+
<strong>Đầu ra:</strong>
+---------+-------------------+
| user_id | confirmation_rate |
+---------+-------------------+
| 6       | 0.00              |
| 3       | 0.00              |
| 7       | 1.00              |
| 2       | 0.50              |
+---------+-------------------+
<strong>Giải thích:</strong>
Người dùng 6 không yêu cầu tin nhắn xác nhận nào. Tỷ lệ xác nhận là 0.
Người dùng 3 đã thực hiện 2 yêu cầu và cả hai đều hết hạn. Tỷ lệ xác nhận là 0.
Người dùng 7 đã thực hiện 3 yêu cầu và tất cả đều được xác nhận. Tỷ lệ xác nhận là 1.
Người dùng 2 đã thực hiện 2 yêu cầu, trong đó một yêu cầu được xác nhận và yêu cầu còn lại hết hạn. Tỷ lệ xác nhận là 1 / 2 = 0.5.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Left Join + Grouping

<!-- thinking:start -->

> **Tư duy**
>
> Tỷ lệ là số action đã được xác nhận chia cho tổng số dòng xác nhận; những người dùng không có dòng nào vẫn phải hiển thị $0.00$.
>
> Dùng left join để nối $\texttt{Confirmations}$ vào $\texttt{Signups}$, group theo $\texttt{user\_id}$, rồi tính tổng có điều kiện chia cho số lượng, dùng $\texttt{IFNULL}$ để điền các giá trị null và làm tròn đến hai chữ số thập phân.

<!-- thinking:end -->

Ta có thể dùng left join để nối bảng `Signups` và bảng `Confirmations` theo `user_id`, sau đó dùng `GROUP BY` để nhóm theo `user_id` nhằm thực hiện phép tổng hợp.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    user_id,
    ROUND(IFNULL(SUM(action = 'confirmed') / COUNT(1), 0), 2) AS confirmation_rate
FROM
    SignUps
    LEFT JOIN Confirmations USING (user_id)
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
