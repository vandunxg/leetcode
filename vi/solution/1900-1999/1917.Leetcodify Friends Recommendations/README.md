---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1917. Leetcodify Friends Recommendations 🔒](https://leetcode.com/problems/leetcodify-friends-recommendations)

[中文文档](/solution/1900-1999/1917.Leetcodify%20Friends%20Recommendations/README.md)

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
Bảng này có thể chứa các bản ghi trùng lặp (nói cách khác, bảng này không có khóa chính trong SQL).
Mỗi hàng trong bảng này cho biết người dùng user_id đã nghe bài hát song_id vào ngày day.
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
Trong SQL, (user1_id, user2_id) là khóa chính của bảng này.
Mỗi hàng trong bảng này cho biết người dùng user1_id và user2_id là bạn bè.
Lưu ý rằng user1_id &lt; user2_id.
</pre>

<p>&nbsp;</p>

<p>Hãy gợi ý bạn bè cho người dùng Leetcodify. Ta gợi ý người dùng <code>x</code> cho người dùng <code>y</code> nếu:</p>

<ul>
	<li>Người dùng <code>x</code> và <code>y</code> không phải là bạn bè, và</li>
	<li>Người dùng <code>x</code> và <code>y</code> đã nghe cùng ít nhất ba bài hát khác nhau <strong>vào cùng một ngày</strong>.</li>
</ul>

<p>Lưu ý rằng việc gợi ý bạn bè là <strong>một chiều</strong>, nghĩa là nếu người dùng <code>x</code> và người dùng <code>y</code> nên được gợi ý cho nhau, bảng kết quả phải chứa cả việc gợi ý người dùng <code>x</code> cho người dùng <code>y</code> và người dùng <code>y</code> cho người dùng <code>x</code>. Ngoài ra, bảng kết quả không được chứa các bản ghi trùng lặp (tức là người dùng <code>y</code> không được gợi ý cho người dùng <code>x</code> nhiều lần).</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

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
+----------+----------+
<strong>Đầu ra:</strong>
+---------+----------------+
| user_id | recommended_id |
+---------+----------------+
| 1       | 3              |
| 2       | 3              |
| 3       | 1              |
| 3       | 2              |
+---------+----------------+
<strong>Giải thích:</strong>
Người dùng 1 và 2 đã nghe các bài hát 10, 11 và 12 vào cùng một ngày, nhưng họ đã là bạn bè.
Người dùng 1 và 3 đã nghe các bài hát 10, 11 và 12 vào cùng một ngày. Vì họ không phải là bạn bè, ta gợi ý họ cho nhau.
Người dùng 1 và 4 không nghe cùng ba bài hát.
Người dùng 1 và 5 đã nghe các bài hát 10, 11 và 12, nhưng vào những ngày khác nhau.

Tương tự, ta thấy người dùng 2 và 3 đã nghe các bài hát 10, 11 và 12 vào cùng một ngày và không phải là bạn bè, nên ta gợi ý họ cho nhau.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một gợi ý cần có ít nhất ba bài hát chung trong cùng một ngày và hai người dùng không được là bạn bè. Tình bạn là quan hệ vô hướng, nên nếu chỉ tạo bảng một chiều thì sẽ bỏ sót cạnh ngược lại.
>
> Ta hợp nhất cả hai hướng của $\texttt{Friendship}$, tự join $\texttt{Listens}$ theo ngày và bài hát với các người dùng khác nhau, rồi loại bỏ các cặp đã là bạn bè.
>
> Nhóm theo ngày và cặp người dùng, đếm số bài hát khác nhau, rồi giữ các nhóm có số lượng ít nhất $3$ để tạo danh sách gợi ý; $\texttt{DISTINCT}$ loại bỏ các bản ghi lặp lại ở những ngày khác nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT user1_id, user2_id FROM Friendship
        UNION
        SELECT user2_id AS user1_id, user1_id AS user2_id FROM Friendship
    )
SELECT DISTINCT l1.user_id, l2.user_id AS recommended_id
FROM
    Listens AS l1,
    Listens AS l2
WHERE
    l1.day = l2.day
    AND l1.song_id = l2.song_id
    AND l1.user_id != l2.user_id
    AND NOT EXISTS (
        SELECT 1
        FROM T AS t
        WHERE l1.user_id = t.user1_id AND l2.user_id = t.user2_id
    )
GROUP BY l1.day, l1.user_id, l2.user_id
HAVING COUNT(DISTINCT l1.song_id) >= 3;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
