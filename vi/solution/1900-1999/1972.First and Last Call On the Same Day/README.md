---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1972. First and Last Call On the Same Day 🔒](https://leetcode.com/problems/first-and-last-call-on-the-same-day)

[中文文档](/solution/1900-1999/1972.First%20and%20Last%20Call%20On%20the%20Same%20Day/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Calls</code></p>

<pre>
+--------------+----------+
| Column Name  | Type     |
+--------------+----------+
| caller_id    | int      |
| recipient_id | int      |
| call_time    | datetime |
+--------------+----------+
(caller_id, recipient_id, call_time) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về thời điểm thực hiện cuộc gọi giữa caller_id và recipient_id.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo ID của những người dùng mà cuộc gọi đầu tiên và cuối cùng trong <strong>bất kỳ ngày nào</strong> đều với <strong>cùng một người</strong>. Các cuộc gọi được tính bất kể người dùng là người gọi hay người nhận.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Calls:
+-----------+--------------+---------------------+
| caller_id | recipient_id | call_time           |
+-----------+--------------+---------------------+
| 8         | 4            | 2021-08-24 17:46:07 |
| 4         | 8            | 2021-08-24 19:57:13 |
| 5         | 1            | 2021-08-11 05:28:44 |
| 8         | 3            | 2021-08-17 04:04:15 |
| 11        | 3            | 2021-08-17 13:07:00 |
| 8         | 11           | 2021-08-17 22:22:22 |
+-----------+--------------+---------------------+
<strong>Đầu ra:</strong>
+---------+
| user_id |
+---------+
| 1       |
| 4       |
| 5       |
| 8       |
+---------+
<strong>Giải thích:</strong>
Vào ngày 2021-08-24, cuộc gọi đầu tiên và cuối cùng trong ngày của người dùng 8 đều là với người dùng 4. Người dùng 8 được đưa vào đáp án.
Tương tự, vào ngày 2021-08-24, cuộc gọi đầu tiên và cuối cùng của người dùng 4 đều là với người dùng 8. Người dùng 4 được đưa vào đáp án.
Vào ngày 2021-08-11, người dùng 1 và 5 có một cuộc gọi. Đây là cuộc gọi duy nhất trong ngày của cả hai người. Vì cuộc gọi này vừa là cuộc gọi đầu tiên vừa là cuộc gọi cuối cùng trong ngày của cả hai, nên cả hai đều được đưa vào đáp án.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một người dùng được chọn nếu cuộc gọi đầu tiên và cuối cùng trong ngày đều với cùng một bên kia. Bảng có hướng; chỉ giữ $\texttt{caller\_id}$ sẽ bỏ sót các cuộc gọi đến.
>
> Gộp thêm các cuộc gọi đảo chiều, sau đó dùng $\texttt{FIRST\_VALUE}$ trên từng cặp (ngày, người dùng), sắp xếp theo thời gian tăng dần và giảm dần. Các cặp đầu mút bằng nhau, sau khi lấy distinct, chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    s AS (
        SELECT
            *
        FROM Calls
        UNION ALL
        SELECT
            recipient_id,
            caller_id,
            call_time
        FROM Calls
    ),
    t AS (
        SELECT
            caller_id user_id,
            FIRST_VALUE(recipient_id) OVER (
                PARTITION BY DATE_FORMAT(call_time, '%Y-%m-%d'), caller_id
                ORDER BY call_time ASC
            ) first,
            FIRST_VALUE(recipient_id) OVER (
                PARTITION BY DATE_FORMAT(call_time, '%Y-%m-%d'), caller_id
                ORDER BY call_time DESC
            ) last
        FROM s
    )
SELECT DISTINCT
    user_id
FROM t
WHERE first = last;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
