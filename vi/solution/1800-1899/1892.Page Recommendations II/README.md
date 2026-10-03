---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1892. Page Recommendations II 🔒](https://leetcode.com/problems/page-recommendations-ii)

[中文文档](/solution/1800-1899/1892.Page%20Recommendations%20II/README.md)

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
(user1_id, user2_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này cho biết người dùng user1_id và user2_id là bạn bè.
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
(user_id, page_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này cho biết user_id thích page_id.
</pre>

<p>&nbsp;</p>

<p>Bạn đang triển khai một hệ thống đề xuất trang cho một website mạng xã hội. Hệ thống sẽ <strong>đề xuất</strong> một trang cho <code>user_id</code> nếu trang đó được <strong>ít nhất một</strong> người bạn của <code>user_id</code> <strong>thích</strong> và <code>user_id</code> <strong>chưa thích</strong> trang đó.</p>

<p>Hãy viết lời giải để tìm tất cả <strong>đề xuất trang</strong> có thể có cho mọi người dùng. Mỗi đề xuất là một hàng trong bảng kết quả với các cột sau:</p>

<ul>
	<li><code>user_id</code>: ID của người dùng được hệ thống đề xuất.</li>
	<li><code>page_id</code>: ID của trang được đề xuất cho <code>user_id</code>.</li>
	<li><code>friends_likes</code>: Số người bạn của <code>user_id</code> thích <code>page_id</code>.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

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
+---------+---------+---------------+
| user_id | page_id | friends_likes |
+---------+---------+---------------+
| 1       | 77      | 2             |
| 1       | 23      | 1             |
| 1       | 24      | 1             |
| 1       | 56      | 1             |
| 1       | 33      | 1             |
| 2       | 24      | 1             |
| 2       | 56      | 1             |
| 2       | 11      | 1             |
| 2       | 88      | 1             |
| 3       | 88      | 1             |
| 3       | 23      | 1             |
| 4       | 88      | 1             |
| 4       | 77      | 1             |
| 4       | 23      | 1             |
| 5       | 77      | 1             |
| 5       | 23      | 1             |
+---------+---------+---------------+
<strong>Giải thích:</strong>
Lấy người dùng 1 làm ví dụ:
  - Người dùng 1 là bạn với người dùng 2, 3, 4 và 6.
  - Các trang được đề xuất là 23 (người dùng 2 thích), 24 (người dùng 3 thích), 56 (người dùng 3 thích), 33 (người dùng 6 thích) và 77 (người dùng 2 và 3 thích).
  - Lưu ý rằng trang 88 không được đề xuất vì người dùng 1 đã thích trang này.

Một ví dụ khác là người dùng 6:
  - Người dùng 6 là bạn với người dùng 1.
  - Người dùng 1 chỉ thích trang 88, nhưng người dùng 6 cũng đã thích trang đó. Vì vậy, người dùng 6 không có đề xuất nào.

Có thể đề xuất trang cho người dùng 2, 3, 4 và 5 theo cách tương tự.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đề xuất những trang mà bạn của người dùng thích nhưng người dùng chưa thích, đồng thời đếm số người bạn thích trang đó. Quan hệ bạn bè là vô hướng, nhưng bảng chỉ lưu một cặp có hướng.
>
> Hợp Friendship với phiên bản đảo ngược của nó, join với Likes để lấy các trang được bạn bè thích, loại những trang người dùng đã thích bằng $\textit{NOT EXISTS}$, rồi nhóm và đếm theo người dùng và trang.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT * FROM Friendship
        UNION
        SELECT user2_id, user1_id FROM Friendship
    )
SELECT user1_id AS user_id, page_id, COUNT(1) AS friends_likes
FROM
    S AS s
    LEFT JOIN Likes AS l ON s.user2_id = l.user_id
WHERE
    NOT EXISTS (
        SELECT 1
        FROM Likes AS l2
        WHERE user1_id = l2.user_id AND l.page_id = l2.page_id
    )
GROUP BY user1_id, page_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
