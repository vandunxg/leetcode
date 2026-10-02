---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1107. New Users Daily Count 🔒](https://leetcode.com/problems/new-users-daily-count)

[中文文档](/solution/1100-1199/1107.New%20Users%20Daily%20Count/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Traffic</code></p>

<pre>
+---------------+---------+
| Tên cột       | Kiểu    |
+---------------+---------+
| user_id       | int     |
| activity      | enum    |
| activity_date | date    |
+---------------+---------+
Bảng này có thể có các hàng trùng lặp.
Cột activity có kiểu ENUM (danh mục) với các giá trị (&#39;login&#39;, &#39;logout&#39;, &#39;jobs&#39;, &#39;groups&#39;, &#39;homepage&#39;).
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo số người dùng đăng nhập lần đầu vào từng ngày trong phạm vi tối đa <code>90</code> ngày tính từ hôm nay. Giả sử hôm nay là <code>2019-06-30</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Traffic:
+---------+----------+---------------+
| user_id | activity | activity_date |
+---------+----------+---------------+
| 1       | login    | 2019-05-01    |
| 1       | homepage | 2019-05-01    |
| 1       | logout   | 2019-05-01    |
| 2       | login    | 2019-06-21    |
| 2       | logout   | 2019-06-21    |
| 3       | login    | 2019-01-01    |
| 3       | jobs     | 2019-01-01    |
| 3       | logout   | 2019-01-01    |
| 4       | login    | 2019-06-21    |
| 4       | groups   | 2019-06-21    |
| 4       | logout   | 2019-06-21    |
| 5       | login    | 2019-03-01    |
| 5       | logout   | 2019-03-01    |
| 5       | login    | 2019-06-21    |
| 5       | logout   | 2019-06-21    |
+---------+----------+---------------+
<strong>Đầu ra:</strong> 
+------------+-------------+
| login_date | user_count  |
+------------+-------------+
| 2019-05-01 | 1           |
| 2019-06-21 | 2           |
+------------+-------------+
<strong>Giải thích:</strong> 
Lưu ý, ta chỉ quan tâm đến những ngày có số người dùng khác 0.
Người dùng có id 5 đăng nhập lần đầu vào ngày 2019-03-01 nên không được tính vào ngày 2019-06-21.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Người dùng mới được xác định theo lần `login` sớm nhất của họ trong `Traffic`. `MIN(activity_date) OVER (PARTITION BY user_id)` tìm ngày đăng nhập đầu tiên của từng người; giữ lại các ngày trong vòng $90$ ngày tính từ `2019-06-30` rồi dùng `COUNT(DISTINCT user_id)` để đếm theo ngày.
>
> Tìm lần đăng nhập đầu tiên trước khi tổng hợp giúp tránh đếm lại những lần đăng nhập sau của cùng một người dùng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            user_id,
            MIN(activity_date) OVER (PARTITION BY user_id) AS login_date
        FROM Traffic
        WHERE activity = 'login'
    )
SELECT login_date, COUNT(DISTINCT user_id) AS user_count
FROM T
WHERE DATEDIFF('2019-06-30', login_date) <= 90
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
