---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i)

[中文文档](/solution/1100-1199/1141.User%20Activity%20for%20the%20Past%2030%20Days%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Activity</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| session_id    | int     |
| activity_date | date    |
| activity_type | enum    |
+---------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Cột activity_type có kiểu ENUM (category), với các giá trị (&#39;open_session&#39;, &#39;end_session&#39;, &#39;scroll_down&#39;, &#39;send_message&#39;).
Bảng này ghi lại hoạt động của người dùng trên một website mạng xã hội. 
Lưu ý rằng mỗi session chỉ thuộc về đúng một người dùng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm số người dùng hoạt động mỗi ngày trong khoảng thời gian <code>30</code> ngày kết thúc vào <code>2019-07-27</code> (tính cả ngày này). Người dùng được xem là hoạt động trong một ngày nếu họ thực hiện ít nhất một hoạt động vào ngày đó.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>Lưu ý: <strong>Bất kỳ</strong> hoạt động nào trong số (<code>&#39;open_session&#39;</code>, <code>&#39;end_session&#39;</code>, <code>&#39;scroll_down&#39;</code>, <code>&#39;send_message&#39;</code>) đều được tính là hoạt động hợp lệ để xem người dùng có hoạt động trong ngày đó hay không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Activity:
+---------+------------+---------------+---------------+
| user_id | session_id | activity_date | activity_type |
+---------+------------+---------------+---------------+
| 1       | 1          | 2019-07-20    | open_session  |
| 1       | 1          | 2019-07-20    | scroll_down   |
| 1       | 1          | 2019-07-20    | end_session   |
| 2       | 4          | 2019-07-20    | open_session  |
| 2       | 4          | 2019-07-21    | send_message  |
| 2       | 4          | 2019-07-21    | end_session   |
| 3       | 2          | 2019-07-21    | open_session  |
| 3       | 2          | 2019-07-21    | send_message  |
| 3       | 2          | 2019-07-21    | end_session   |
| 4       | 3          | 2019-06-25    | open_session  |
| 4       | 3          | 2019-06-25    | end_session   |
+---------+------------+---------------+---------------+
<strong>Đầu ra:</strong> 
+------------+--------------+ 
| day        | active_users |
+------------+--------------+ 
| 2019-07-20 | 2            |
| 2019-07-21 | 2            |
+------------+--------------+ 
<strong>Giải thích:</strong> Không cần đưa vào các ngày không có người dùng hoạt động.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số người dùng khác nhau mỗi ngày trong khoảng $30$ ngày kết thúc vào `2019-07-27`. Mệnh đề `WHERE` giới hạn khoảng ngày; sau đó dùng `GROUP BY` theo ngày và `COUNT(DISTINCT user_id)` để đếm.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date <= '2019-07-27' AND DATEDIFF('2019-07-27', activity_date) < 30
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
