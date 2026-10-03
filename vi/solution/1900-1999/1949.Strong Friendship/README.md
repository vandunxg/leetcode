---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1949. Strong Friendship 🔒](https://leetcode.com/problems/strong-friendship)

[中文文档](/solution/1900-1999/1949.Strong%20Friendship/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Friendship</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user1_id    | int  |
| user2_id    | int  |
+-------------+------+
(user1_id, user2_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này cho biết người dùng user1_id và user2_id là bạn bè.
Lưu ý rằng user1_id &lt; user2_id.
</pre>

<p>&nbsp;</p>

<p>Tình bạn giữa một cặp bạn bè <code>x</code> và <code>y</code> được gọi là <strong>bền chặt</strong> nếu <code>x</code> và <code>y</code> có <strong>ít nhất ba</strong> người bạn chung.</p>

<p>Hãy viết lời giải để tìm tất cả <strong>tình bạn bền chặt</strong>.</p>

<p>Lưu ý rằng bảng kết quả không được chứa các bản ghi trùng lặp và phải thỏa mãn <code>user1_id &lt; user2_id</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

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
| 2        | 3        |
| 1        | 4        |
| 2        | 4        |
| 1        | 5        |
| 2        | 5        |
| 1        | 7        |
| 3        | 7        |
| 1        | 6        |
| 3        | 6        |
| 2        | 6        |
+----------+----------+
<strong>Đầu ra:</strong>
+----------+----------+---------------+
| user1_id | user2_id | common_friend |
+----------+----------+---------------+
| 1        | 2        | 4             |
| 1        | 3        | 3             |
+----------+----------+---------------+
<strong>Giải thích:</strong>
Người dùng 1 và 2 có 4 người bạn chung (3, 4, 5 và 6).
Người dùng 1 và 3 có 3 người bạn chung (2, 6 và 7).
Không đưa tình bạn giữa người dùng 2 và 3 vào kết quả vì họ chỉ có hai người bạn chung (1 và 6).
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp bạn bền chặt là hai người bạn có ít nhất ba người bạn chung. Bảng này biểu diễn quan hệ vô hướng, vì vậy chúng ta cần xét cả hai hướng.
>
> Gộp các cạnh đảo chiều, sau đó join ba bản sao để $u\to v$, $v\to w$ và $u\to w$ tạo thành một tam giác, đồng thời giữ $u<v$ để tránh trùng lặp.
>
> Đếm số hàng xóm chung của mỗi cặp, rồi giữ lại các cặp có số lượng ít nhất là $3$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            *
        FROM Friendship
        UNION ALL
        SELECT
            user2_id,
            user1_id
        FROM Friendship
    )
SELECT
    t1.user1_id,
    t1.user2_id,
    COUNT(1) AS common_friend
FROM
    t AS t1
    JOIN t AS t2 ON t1.user2_id = t2.user1_id
    JOIN t AS t3 ON t1.user1_id = t3.user1_id
WHERE t3.user2_id = t2.user2_id AND t1.user1_id < t1.user2_id
GROUP BY t1.user1_id, t1.user2_id
HAVING COUNT(1) >= 3;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
