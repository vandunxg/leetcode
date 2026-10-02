---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1241. Number of Comments per Post 🔒](https://leetcode.com/problems/number-of-comments-per-post)

[中文文档](/solution/1200-1299/1241.Number%20of%20Comments%20per%20Post/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Submissions</code></p>

<pre>
+---------------+----------+
| Column Name   | Type     |
+---------------+----------+
| sub_id        | int      |
| parent_id     | int      |
+---------------+----------+
Bảng này có thể chứa các hàng trùng lặp.
Mỗi hàng có thể là một bài đăng hoặc bình luận thuộc bài đăng đó.
Với bài đăng, parent_id có giá trị null.
Với bình luận, parent_id là <code>sub_id</code> của một bài đăng khác trong bảng.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm số bình luận của mỗi bài đăng. Bảng kết quả cần có <code>post_id</code> và <code>number_of_comments</code> tương ứng.</p>

<p>Bảng <code>Submissions</code> có thể chứa bình luận trùng lặp. Hãy đếm số <strong>bình luận duy nhất</strong> của mỗi bài đăng.</p>

<p>Bảng <code>Submissions</code> có thể chứa bài đăng trùng lặp. Hãy xem chúng là cùng một bài đăng.</p>

<p>Bảng kết quả cần được <strong>sắp xếp</strong> theo <code>post_id</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Submissions:
+---------+------------+
| sub_id  | parent_id  |
+---------+------------+
| 1       | Null       |
| 2       | Null       |
| 1       | Null       |
| 12      | Null       |
| 3       | 1          |
| 5       | 2          |
| 3       | 1          |
| 4       | 1          |
| 9       | 1          |
| 10      | 2          |
| 6       | 7          |
+---------+------------+
<strong>Đầu ra:</strong> 
+---------+--------------------+
| post_id | number_of_comments |
+---------+--------------------+
| 1       | 3                  |
| 2       | 2                  |
| 12      | 0                  |
+---------+--------------------+
<strong>Giải thích:</strong> 
Bài đăng có id 1 có ba bình luận trong bảng, với id 3, 4 và 9. Bình luận có id 3 xuất hiện lặp lại trong bảng nên chỉ được tính <strong>một lần</strong>.
Bài đăng có id 2 có hai bình luận trong bảng, với id 5 và 10.
Bài đăng có id 12 không có bình luận nào trong bảng.
Bình luận có id 6 thuộc về bài đăng đã bị xóa có id 7 nên không được tính.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Bài đăng là một submission có $parent\_id$ bằng null; bình luận trỏ đến $sub\_id$ của một bài đăng. Self-join ghép mỗi bài đăng với các bình luận con, còn left join vẫn giữ lại bài đăng không có bình luận. Sau khi loại trùng bằng $DISTINCT$, ta đếm theo bài đăng để một bình luận xuất hiện nhiều lần chỉ được tính một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT DISTINCT s1.sub_id AS post_id, s2.sub_id AS sub_id
        FROM
            Submissions AS s1
            LEFT JOIN Submissions AS s2 ON s1.sub_id = s2.parent_id
        WHERE s1.parent_id IS NULL
    )
SELECT post_id, COUNT(sub_id) AS number_of_comments
FROM t
GROUP BY post_id
ORDER BY post_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
