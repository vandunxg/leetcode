---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1264. Page Recommendations 🔒](https://leetcode.com/problems/page-recommendations)

[中文文档](/solution/1200-1299/1264.Page%20Recommendations/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Friendship</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user1_id      | int     |
| user2_id      | int     |
+---------------+---------+
(user1_id, user2_id) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng trong bảng biểu thị quan hệ bạn bè giữa user1_id và user2_id.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Likes</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| page_id     | int     |
+-------------+---------+
(user_id, page_id) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng trong bảng cho biết user_id thích page_id.
</pre>

<p>&nbsp;</p>

<p>Viết truy vấn đề xuất cho người dùng có <code>user_id = 1</code> những page mà bạn bè của họ đã thích. Không đề xuất lại các page mà người dùng này đã thích.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong> và không có giá trị trùng lặp.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Friendship:
+----------+----------+
| user1_id | user2_id |
+----------+----------+
| 1        | 2        |
| 1        | 3        |
| 1        | 4        |
| 2        | 3        |
| 2        | 4        |
| 2        | 5        |
| 6        | 1        |
+----------+----------+
Bảng Likes:
+---------+---------+
| user_id | page_id |
+---------+---------+
| 1       | 88      |
| 2       | 23      |
| 3       | 24      |
| 4       | 56      |
| 5       | 11      |
| 6       | 33      |
| 2       | 77      |
| 3       | 77      |
| 6       | 88      |
+---------+---------+
<strong>Đầu ra:</strong> 
+------------------+
| recommended_page |
+------------------+
| 23               |
| 24               |
| 56               |
| 33               |
| 77               |
+------------------+
<strong>Giải thích:</strong> 
Người dùng 1 là bạn của các người dùng 2, 3, 4 và 6.
Các page được đề xuất gồm 23 từ người dùng 2, 24 và 56 từ người dùng 3, và 33 từ người dùng 6.
Page 77 được cả người dùng 2 và người dùng 3 đề xuất.
Page 88 không được đề xuất vì người dùng 1 đã thích page này.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union + Equi-Join + Subquery

<!-- thinking:start -->

> **Tư duy**
>
> Đề xuất là page mà bạn bè của người dùng $1$ thích nhưng người dùng $1$ chưa thích. Quan hệ bạn bè là vô hướng, nên ta union cả hai đầu của các quan hệ có liên quan đến người dùng $1$, join với $Likes$, rồi loại các $page\_id$ mà người dùng này đã thích.

<!-- thinking:end -->

Trước tiên, ta query tất cả người dùng là bạn của `user_id = 1` và lưu họ vào bảng `T`. Sau đó, ta query các page mà người dùng trong bảng `T` đã thích, rồi loại những page mà `user_id = 1` đã thích.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT user1_id AS user_id FROM Friendship WHERE user2_id = 1
        UNION
        SELECT user2_id AS user_id FROM Friendship WHERE user1_id = 1
    )
SELECT DISTINCT page_id AS recommended_page
FROM
    T
    JOIN Likes USING (user_id)
WHERE page_id NOT IN (SELECT page_id FROM Likes WHERE user_id = 1);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo danh sách bạn bè rồi join. Ta có thể đặt truy vấn con tìm bạn bè trực tiếp trong điều kiện $IN$ trên $Likes.user\_id$, nhờ đó bỏ được CTE. Ý nghĩa vẫn như nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DISTINCT page_id AS recommended_page
FROM Likes
WHERE
    user_id IN (
        SELECT user1_id AS user_id FROM Friendship WHERE user2_id = 1
        UNION ALL
        SELECT user2_id AS user_id FROM Friendship WHERE user1_id = 1
    )
    AND page_id NOT IN (SELECT page_id FROM Likes WHERE user_id = 1);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
