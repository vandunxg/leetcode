---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1341. Movie Rating](https://leetcode.com/problems/movie-rating)

[中文文档](/solution/1300-1399/1341.Movie%20Rating/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Movies</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| title         | varchar |
+---------------+---------+
movie_id là primary key (cột có giá trị duy nhất) của bảng này.
title là tên phim.
Mỗi phim có title duy nhất.</pre>

<p>Bảng: <code>Users</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
+---------------+---------+
user_id là primary key (cột có giá trị duy nhất) của bảng này.
Cột &#39;name&#39; có các giá trị duy nhất.
</pre>

<p>Bảng: <code>MovieRating</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| user_id       | int     |
| rating        | int     |
| created_at    | date    |
+---------------+---------+
(movie_id, user_id) là primary key (tổ hợp cột có giá trị duy nhất) của bảng này.
Bảng này lưu đánh giá của người dùng dành cho một bộ phim trong bài review của họ.
created_at là ngày người dùng viết review. 
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để:</p>

<ul>
	<li>Tìm tên người dùng đã đánh giá nhiều phim nhất. Nếu hòa, trả về tên người dùng nhỏ hơn theo thứ tự từ điển.</li>
	<li>Tìm tên phim có rating trung bình <strong>cao nhất</strong> trong <code>February 2020</code>. Nếu hòa, trả về tên phim nhỏ hơn theo thứ tự từ điển.</li>
</ul>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Movies:
+-------------+--------------+
| movie_id    |  title       |
+-------------+--------------+
| 1           | Avengers     |
| 2           | Frozen 2     |
| 3           | Joker        |
+-------------+--------------+
Bảng Users:
+-------------+--------------+
| user_id     |  name        |
+-------------+--------------+
| 1           | Daniel       |
| 2           | Monica       |
| 3           | Maria        |
| 4           | James        |
+-------------+--------------+
Bảng MovieRating:
+-------------+--------------+--------------+-------------+
| movie_id    | user_id      | rating       | created_at  |
+-------------+--------------+--------------+-------------+
| 1           | 1            | 3            | 2020-01-12  |
| 1           | 2            | 4            | 2020-02-11  |
| 1           | 3            | 2            | 2020-02-12  |
| 1           | 4            | 1            | 2020-01-01  |
| 2           | 1            | 5            | 2020-02-17  | 
| 2           | 2            | 2            | 2020-02-01  | 
| 2           | 3            | 2            | 2020-03-01  |
| 3           | 1            | 3            | 2020-02-22  | 
| 3           | 2            | 4            | 2020-02-25  | 
+-------------+--------------+--------------+-------------+
<strong>Đầu ra:</strong> 
+--------------+
| results      |
+--------------+
| Daniel       |
| Frozen 2     |
+--------------+
<strong>Giải thích:</strong> 
Daniel và Monica đều đã đánh giá 3 phim (&quot;Avengers&quot;, &quot;Frozen 2&quot; và &quot;Joker&quot;), nhưng Daniel nhỏ hơn theo thứ tự từ điển.
Frozen 2 và Joker có rating trung bình trong tháng 2 là 3.5, nhưng Frozen 2 nhỏ hơn theo thứ tự từ điển.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Gộp hai giá trị kết quả thành một cột: người dùng có nhiều lượt đánh giá nhất (nếu hòa, chọn tên nhỏ nhất theo thứ tự từ điển) và phim có rating trung bình cao nhất trong tháng 2 năm $2020$ (cùng quy tắc phá hòa). Mỗi truy vấn lần lượt join, group, sắp xếp và lấy $\mathrm{LIMIT}\,1$; $\mathrm{UNION\ ALL}$ nối hai kết quả lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
(
    SELECT name AS results
    FROM
        Users
        JOIN MovieRating USING (user_id)
    GROUP BY user_id
    ORDER BY COUNT(1) DESC, name
    LIMIT 1
)
UNION ALL
(
    SELECT title
    FROM
        MovieRating
        JOIN Movies USING (movie_id)
    WHERE DATE_FORMAT(created_at, '%Y-%m') = '2020-02'
    GROUP BY movie_id
    ORDER BY AVG(rating) DESC, title
    LIMIT 1
);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
