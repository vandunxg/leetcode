---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3172. Second Day Verification 🔒](https://leetcode.com/problems/second-day-verification)

[中文文档](/solution/3100-3199/3172.Second%20Day%20Verification/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>emails</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| email_id    | int      |
| user_id     | int      |
| signup_date | datetime |
+-------------+----------+
(email_id, user_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này chứa ID email, ID người dùng và ngày đăng ký.
</pre>

<p>Bảng: <code>texts</code></p>

<pre>
+---------------+----------+
| Column Name   | Type     |
+---------------+----------+
| text_id       | int      |
| email_id      | int      |
| signup_action | enum     |
| action_date   | datetime |
+---------------+----------+
(text_id, email_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
signup_action là kiểu enum gồm (&#39;Verified&#39;, &#39;Not Verified&#39;).
Mỗi hàng trong bảng này chứa ID tin nhắn, ID email, thao tác đăng ký và ngày thực hiện thao tác.
</pre>

<p>Viết một lời giải để tìm ID người dùng của những người đã <strong>xác minh</strong> việc <strong>đăng ký</strong> vào <strong>ngày thứ hai</strong>.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>user_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng emails:</p>

<pre class="example-io">
+----------+---------+---------------------+
| email_id | user_id | signup_date         |
+----------+---------+---------------------+
| 125      | 7771    | 2022-06-14 09:30:00|
| 433      | 1052    | 2022-07-09 08:15:00|
| 234      | 7005    | 2022-08-20 10:00:00|
+----------+---------+---------------------+
</pre>

<p>bảng texts:</p>

<pre class="example-io">
+---------+----------+--------------+---------------------+
| text_id | email_id | signup_action| action_date         |
+---------+----------+--------------+---------------------+
| 1       | 125      | Verified     | 2022-06-15 08:30:00|
| 2       | 433      | Not Verified | 2022-07-10 10:45:00|
| 4       | 234      | Verified     | 2022-08-21 09:30:00|
+---------+----------+--------------+---------------------+
    </pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+
| user_id |
+---------+
| 7005    |
| 7771    |
+---------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Người dùng có user_id 7005 và email_id 234 đã đăng ký vào ngày 2022-08-20 10:00:00 và&nbsp;xác minh vào ngày thứ hai kể từ khi đăng ký.</li>
	<li>Người dùng có user_id 7771 và email_id 125 đã đăng ký vào ngày 2022-06-14 09:30:00 và&nbsp;xác minh vào ngày thứ hai kể từ khi đăng ký.</li>
</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Kết hợp hai bảng

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm những người dùng đã xác minh vào ngày sau ngày đăng ký. Dữ liệu email và tin nhắn nằm trong hai bảng.
>
> Phép inner join trên $email\_id$ với $DATEDIFF=1$ và `signup_action='Verified'` áp dụng đồng thời mọi điều kiện.
>
> Chọn $user\_id$ theo thứ tự tăng dần. Không cần nhóm kết quả.

<!-- thinking:end -->

Chúng ta có thể kết hợp hai bảng, sau đó dùng hàm `DATEDIFF` để kiểm tra xem chênh lệch giữa ngày đăng ký và ngày thực hiện thao tác có bằng 1 hay không, đồng thời kiểm tra thao tác đăng ký có phải là `Verified` hay không, từ đó lọc ra các ID người dùng thỏa mãn điều kiện.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT user_id
FROM
    Emails AS e
    JOIN texts AS t
        ON e.email_id = t.email_id
        AND DATEDIFF(action_date, signup_date) = 1
        AND signup_action = 'Verified'
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
