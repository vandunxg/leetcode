---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies)

[中文文档](/solution/0600-0699/0620.Not%20Boring%20Movies/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Cinema</code></p>

<pre>
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| id             | int      |
| movie          | varchar  |
| description    | varchar  |
| rating         | float    |
+----------------+----------+
id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về tên phim, thể loại và điểm đánh giá.
rating là số thực có 2 chữ số thập phân, nằm trong khoảng [0, 10].
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm các bộ phim có ID lẻ và description khác <code>&quot;boring&quot;</code>.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>rating</code> <strong>theo thứ tự giảm dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Cinema:
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 1  | War        | great 3D    | 8.9    |
| 2  | Science    | fiction     | 8.5    |
| 3  | irish      | boring      | 6.2    |
| 4  | Ice song   | Fantacy     | 8.6    |
| 5  | House card | Interesting | 9.1    |
+----+------------+-------------+--------+
<strong>Đầu ra:</strong> 
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |
+----+------------+-------------+--------+
<strong>Giải thích:</strong> 
Có ba bộ phim có ID lẻ: 1, 3 và 5. Phim có ID = 3 nhàm chán nên không được đưa vào kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lọc theo điều kiện + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Hai điều kiện lọc độc lập: description khác `boring` và `id` là số lẻ; sau đó sắp xếp theo rating.
>
> Chỉ cần truy vấn `WHERE description != 'boring' AND id & 1 = 1` kết hợp với `ORDER BY rating DESC`.

<!-- thinking:end -->

Ta có thể dùng mệnh đề `WHERE` để lọc các hàng có `description` khác `boring` và `id` là số lẻ, sau đó dùng `ORDER BY` để sắp xếp kết quả theo `rating` giảm dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT *
FROM Cinema
WHERE description != 'boring' AND id & 1 = 1
ORDER BY 4 DESC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
