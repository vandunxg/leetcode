---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1951. All the Pairs With the Maximum Number of Common Followers 🔒](https://leetcode.com/problems/all-the-pairs-with-the-maximum-number-of-common-followers)

[中文文档](/solution/1900-1999/1951.All%20the%20Pairs%20With%20the%20Maximum%20Number%20of%20Common%20Followers/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Relations</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| follower_id | int  |
+-------------+------+
(user_id, follower_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này cho biết người dùng có ID follower_id đang theo dõi người dùng có ID user_id.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm tất cả các cặp người dùng có số người theo dõi chung lớn nhất. Nói cách khác, nếu số người theo dõi chung lớn nhất giữa bất kỳ hai người dùng nào là <code>maxCommon</code>, thì cần trả về tất cả các cặp người dùng có <code>maxCommon</code> người theo dõi chung.</p>

<p>Bảng kết quả cần chứa các cặp <code>user1_id</code> và <code>user2_id</code> sao cho <code>user1_id &lt; user2_id</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Relations:
+---------+-------------+
| user_id | follower_id |
+---------+-------------+
| 1       | 3           |
| 2       | 3           |
| 7       | 3           |
| 1       | 4           |
| 2       | 4           |
| 7       | 4           |
| 1       | 5           |
| 2       | 6           |
| 7       | 5           |
+---------+-------------+
<strong>Đầu ra:</strong>
+----------+----------+
| user1_id | user2_id |
+----------+----------+
| 1        | 7        |
+----------+----------+
<strong>Giải thích:</strong>
Người dùng 1 và 2 có hai người theo dõi chung (3 và 4).
Người dùng 1 và 7 có ba người theo dõi chung (3, 4 và 5).
Người dùng 2 và 7 có hai người theo dõi chung (3 và 4).
Vì số người theo dõi chung lớn nhất giữa bất kỳ hai người dùng nào là 3, ta trả về tất cả các cặp người dùng có ba người theo dõi chung, trong trường hợp này chỉ có cặp (1, 7). Cặp được trả về là (1, 7), không phải (7, 1).
Lưu ý rằng chúng ta không có thông tin về những người dùng theo dõi người dùng 3, 4 và 5, nên xem như họ có 0 người theo dõi.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần mọi cặp người dùng đạt số người theo dõi chung lớn nhất. Không cần liệt kê các cặp bên ngoài SQL.
>
> Self-join $\texttt{Relations}$ theo $\textit{follower\_id}$ với $user_1<user_2$, đếm theo từng cặp, xếp hạng theo số lượng giảm dần, rồi giữ lại hạng $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            r1.user_id AS user1_id,
            r2.user_id AS user2_id,
            RANK() OVER (ORDER BY COUNT(1) DESC) AS rk
        FROM
            Relations AS r1
            JOIN Relations AS r2 ON r1.follower_id = r2.follower_id AND r1.user_id < r2.user_id
        GROUP BY r1.user_id, r2.user_id
    )
SELECT
    user1_id,
    user2_id
FROM t
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
