---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1890. The Latest Login in 2020](https://leetcode.com/problems/the-latest-login-in-2020)

[中文文档](/solution/1800-1899/1890.The%20Latest%20Login%20in%202020/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Logins</code></p>

<pre>
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
(user_id, time_stamp) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về thời điểm đăng nhập của người dùng có ID user_id.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo lần đăng nhập <strong>gần nhất</strong> của tất cả người dùng trong năm <code>2020</code>. <strong>Không</strong> đưa vào những người dùng không đăng nhập trong năm <code>2020</code>.</p>

<p>Có thể trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Logins:
+---------+---------------------+
| user_id | time_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 6       | 2021-04-21 14:06:06 |
| 6       | 2019-03-07 00:18:15 |
| 8       | 2020-02-01 05:10:53 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
| 2       | 2019-08-25 07:59:08 |
| 14      | 2019-07-14 09:00:00 |
| 14      | 2021-01-06 11:59:59 |
+---------+---------------------+
<strong>Đầu ra:</strong>
+---------+---------------------+
| user_id | last_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
+---------+---------------------+
<strong>Giải thích:</strong>
Người dùng 6 đã đăng nhập tài khoản 3 lần nhưng chỉ một lần trong năm 2020, nên ta đưa lần đăng nhập này vào bảng kết quả.
Người dùng 8 đã đăng nhập tài khoản 2 lần trong năm 2020, một lần vào tháng 2 và một lần vào tháng 12. Ta chỉ đưa lần gần nhất (tháng 12) vào bảng kết quả.
Người dùng 2 đã đăng nhập tài khoản 2 lần nhưng chỉ một lần trong năm 2020, nên ta đưa lần đăng nhập này vào bảng kết quả.
Người dùng 14 không đăng nhập trong năm 2020, nên ta không đưa người dùng này vào bảng kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By + Hàm Max

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần lần đăng nhập cuối cùng của mỗi người dùng trong năm $2020$. Lọc theo năm, sau đó lấy timestamp lớn nhất của từng người dùng.
>
> Dùng $\textit{WHERE YEAR}(time\_stamp)=2020$, $\textit{GROUP BY}$ $user\_id$ và $MAX(time\_stamp)$.

<!-- thinking:end -->

Trước tiên, ta lọc các bản ghi đăng nhập trong năm 2020, sau đó nhóm theo `user_id` và dùng hàm `max` để tính thời điểm đăng nhập lớn nhất của mỗi người dùng.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT user_id, MAX(time_stamp) AS last_stamp
FROM Logins
WHERE YEAR(time_stamp) = 2020
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
