---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1919. Leetcodify Similar Friends 🔒](https://leetcode.com/problems/leetcodify-similar-friends)

[中文文档](/solution/1900-1999/1919.Leetcodify%20Similar%20Friends/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Listens</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| song_id     | int     |
| day         | date    |
+-------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Mỗi hàng của bảng này cho biết người dùng user_id đã nghe bài hát song_id vào ngày day.
</pre>

<p>&nbsp;</p>

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
Lưu ý rằng user1_id &lt; user2_id.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo những người bạn tương đồng của người dùng Leetcodify. Người dùng <code>x</code> và người dùng <code>y</code> là những người bạn tương đồng nếu:</p>

<ul>
	<li>Người dùng <code>x</code> và người dùng <code>y</code> là bạn bè, và</li>
	<li>Người dùng <code>x</code> và người dùng <code>y</code> đã nghe cùng ít nhất ba bài hát khác nhau <strong>vào cùng một ngày</strong>.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>. Lưu ý rằng bạn phải trả về các cặp bạn bè tương đồng theo đúng cách chúng được biểu diễn trong đầu vào (tức là luôn <code>user1_id &lt; user2_id</code>).</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Listens:
+---------+---------+------------+
| user_id | song_id | day        |
+---------+---------+------------+
| 1       | 10      | 2021-03-15 |
| 1       | 11      | 2021-03-15 |
| 1       | 12      | 2021-03-15 |
| 2       | 10      | 2021-03-15 |
| 2       | 11      | 2021-03-15 |
| 2       | 12      | 2021-03-15 |
| 3       | 10      | 2021-03-15 |
| 3       | 11      | 2021-03-15 |
| 3       | 12      | 2021-03-15 |
| 4       | 10      | 2021-03-15 |
| 4       | 11      | 2021-03-15 |
| 4       | 13      | 2021-03-15 |
| 5       | 10      | 2021-03-16 |
| 5       | 11      | 2021-03-16 |
| 5       | 12      | 2021-03-16 |
+---------+---------+------------+
Bảng Friendship:
+----------+----------+
| user1_id | user2_id |
+----------+----------+
| 1        | 2        |
| 2        | 4        |
| 2        | 5        |
+----------+----------+
<strong>Đầu ra:</strong>
+----------+----------+
| user1_id | user2_id |
+----------+----------+
| 1        | 2        |
+----------+----------+
<strong>Giải thích:</strong>
Người dùng 1 và 2 là bạn bè, đồng thời đã nghe các bài hát 10, 11 và 12 vào cùng một ngày. Họ là những người bạn tương đồng.
Người dùng 1 và 3 đã nghe các bài hát 10, 11 và 12 vào cùng một ngày, nhưng họ không phải bạn bè.
Người dùng 2 và 4 là bạn bè, nhưng họ không nghe cùng ba bài hát khác nhau.
Người dùng 2 và 5 là bạn bè và đã nghe các bài hát 10, 11 và 12, nhưng họ không nghe chúng vào cùng một ngày.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Những người bạn tương đồng vốn đã là bạn bè và cùng nghe ít nhất ba bài hát khác nhau vào một ngày nào đó. Những người không phải bạn bè không liên quan.
>
> Nối $\texttt{Friendship}$ với các hàng $\texttt{Listens}$ của cả hai người dùng theo cùng ngày và bài hát, sau đó nhóm theo cặp bạn bè và ngày.
>
> Giữ lại các cặp có số bài hát khác nhau ít nhất là $3$; $\texttt{DISTINCT}$ loại bỏ các bản sao giữa các ngày.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DISTINCT user1_id, user2_id
FROM
    Friendship AS f
    LEFT JOIN Listens AS l1 ON user1_id = l1.user_id
    LEFT JOIN Listens AS l2 ON user2_id = l2.user_id
WHERE l1.song_id = l2.song_id AND l1.day = l2.day
GROUP BY 1, 2, l1.day
HAVING COUNT(DISTINCT l1.song_id) >= 3;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
