---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1132. Reported Posts II 🔒](https://leetcode.com/problems/reported-posts-ii)

[中文文档](/solution/1100-1199/1132.Reported%20Posts%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Actions</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| post_id       | int     |
| action_date   | date    | 
| action        | enum    |
| extra         | varchar |
+---------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Cột action có kiểu ENUM (category), gồm các giá trị (&#39;view&#39;, &#39;like&#39;, &#39;reaction&#39;, &#39;comment&#39;, &#39;report&#39;, &#39;share&#39;).
Cột extra chứa thông tin tùy chọn về hành động, chẳng hạn lý do report hoặc loại reaction.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Removals</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| post_id       | int     |
| remove_date   | date    | 
+---------------+---------+
post_id là primary key (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết một bài đăng đã bị gỡ do bị report hoặc sau khi admin xem xét.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm tỷ lệ phần trăm trung bình mỗi ngày của các bài đăng bị gỡ sau khi được report là spam, <strong>làm tròn đến 2 chữ số thập phân</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Actions:
+---------+---------+-------------+--------+--------+
| user_id | post_id | action_date | action | extra  |
+---------+---------+-------------+--------+--------+
| 1       | 1       | 2019-07-01  | view   | null   |
| 1       | 1       | 2019-07-01  | like   | null   |
| 1       | 1       | 2019-07-01  | share  | null   |
| 2       | 2       | 2019-07-04  | view   | null   |
| 2       | 2       | 2019-07-04  | report | spam   |
| 3       | 4       | 2019-07-04  | view   | null   |
| 3       | 4       | 2019-07-04  | report | spam   |
| 4       | 3       | 2019-07-02  | view   | null   |
| 4       | 3       | 2019-07-02  | report | spam   |
| 5       | 2       | 2019-07-03  | view   | null   |
| 5       | 2       | 2019-07-03  | report | racism |
| 5       | 5       | 2019-07-03  | view   | null   |
| 5       | 5       | 2019-07-03  | report | racism |
+---------+---------+-------------+--------+--------+
Bảng Removals:
+---------+-------------+
| post_id | remove_date |
+---------+-------------+
| 2       | 2019-07-20  |
| 3       | 2019-07-18  |
+---------+-------------+
<strong>Đầu ra:</strong> 
+-----------------------+
| average_daily_percent |
+-----------------------+
| 75.00                 |
+-----------------------+
<strong>Giải thích:</strong> 
Tỷ lệ phần trăm của ngày 2019-07-04 là 50% vì chỉ một trong hai bài đăng bị report spam đã bị gỡ.
Tỷ lệ phần trăm của ngày 2019-07-02 là 100% vì có một bài đăng bị report là spam và bài đó đã bị gỡ.
Các ngày còn lại không có report spam, nên giá trị trung bình là (50 + 100) / 2 = 75%.
Lưu ý rằng output chỉ gồm một con số và ta không cần quan tâm đến ngày gỡ bài.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tính tỷ lệ bài đăng spam bị gỡ trên tổng số bài đăng bị report spam của từng ngày, rồi lấy trung bình các tỷ lệ đó. Group bảng `Actions` với `extra='spam'` theo `action_date`, left join `Removals`, chia hai giá trị `COUNT(DISTINCT post_id)`, sau đó dùng `AVG` lấy trung bình các tỷ lệ ngày và làm tròn.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            COUNT(DISTINCT t2.post_id) / COUNT(DISTINCT t1.post_id) * 100 AS percent
        FROM
            Actions AS t1
            LEFT JOIN Removals AS t2 ON t1.post_id = t2.post_id
        WHERE extra = 'spam'
        GROUP BY action_date
    )
SELECT ROUND(AVG(percent), 2) AS average_daily_percent
FROM T;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
