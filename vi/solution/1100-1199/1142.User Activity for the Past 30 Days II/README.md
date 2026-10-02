---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1142. User Activity for the Past 30 Days II 🔒](https://leetcode.com/problems/user-activity-for-the-past-30-days-ii)

[中文文档](/solution/1100-1199/1142.User%20Activity%20for%20the%20Past%2030%20Days%20II/README.md)

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
Cột activity_type có kiểu ENUM (danh mục) với các giá trị (&#39;open_session&#39;, &#39;end_session&#39;, &#39;scroll_down&#39;, &#39;send_message&#39;).
Bảng ghi lại hoạt động của người dùng trên một trang mạng xã hội. 
Lưu ý, mỗi session chỉ thuộc về đúng một người dùng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm số session trung bình trên mỗi người dùng trong khoảng thời gian <code>30</code> ngày kết thúc vào <code>2019-07-27</code> (tính cả ngày này), rồi <strong>làm tròn đến 2 chữ số thập phân</strong>. Chỉ tính những session có ít nhất một hoạt động trong khoảng thời gian đó.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Activity table:
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
| 3       | 5          | 2019-07-21    | open_session  |
| 3       | 5          | 2019-07-21    | scroll_down   |
| 3       | 5          | 2019-07-21    | end_session   |
| 4       | 3          | 2019-06-25    | open_session  |
| 4       | 3          | 2019-06-25    | end_session   |
+---------+------------+---------------+---------------+
<strong>Output:</strong> 
+---------------------------+ 
| average_sessions_per_user |
+---------------------------+ 
| 1.33                      |
+---------------------------+
<strong>Giải thích:</strong> Người dùng 1 và 2 mỗi người có 1 session trong 30 ngày qua, còn người dùng 3 có 2 session, nên giá trị trung bình là (1 + 1 + 2) / 3 = 1.33.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số `session_id` khác nhau của mỗi người dùng trong khoảng ngày, rồi tính trung bình các số lượng session đó. `IFNULL` trả về $0$ nếu bảng kết quả rỗng. Khoảng thời gian giống bài trước.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            COUNT(DISTINCT session_id) AS sessions
        FROM Activity
        WHERE activity_date <= '2019-07-27' AND DATEDIFF('2019-07-27', activity_date) < 30
        GROUP BY user_id
    )
SELECT IFNULL(ROUND(AVG(sessions), 2), 0) AS average_sessions_per_user
FROM T;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tính trung bình số session theo từng người dùng. Vì mỗi session chỉ thuộc về một người dùng, trên schema này `COUNT(DISTINCT session_id)/COUNT(DISTINCT user_id)` cho cùng kết quả trung bình và không cần dùng CTE.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    IFNULL(
        ROUND(COUNT(DISTINCT session_id) / COUNT(DISTINCT user_id), 2),
        0
    ) AS average_sessions_per_user
FROM Activity
WHERE DATEDIFF('2019-07-27', activity_date) < 30;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
