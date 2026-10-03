---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1811. Find Interview Candidates 🔒](https://leetcode.com/problems/find-interview-candidates)

[中文文档](/solution/1800-1899/1811.Find%20Interview%20Candidates/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Contests</code></p>

<pre>
+--------------+------+
| Column Name  | Type |
+--------------+------+
| contest_id   | int  |
| gold_medal   | int  |
| silver_medal | int  |
| bronze_medal | int  |
+--------------+------+
contest_id là cột chứa các giá trị duy nhất trong bảng này.
Bảng này chứa ID cuộc thi LeetCode và ID người dùng của những người đạt huy chương vàng, bạc và đồng.
It is guaranteed that any consecutive contests have consecutive IDs and that no ID is skipped.</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Users</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| mail        | varchar |
| name        | varchar |
+-------------+---------+
user_id là cột chứa các giá trị duy nhất trong bảng này.
Bảng này chứa thông tin về người dùng.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo <code>name</code> và <code>mail</code> của tất cả <strong>ứng viên phỏng vấn</strong>. Một người dùng là <strong>ứng viên phỏng vấn</strong> nếu <strong>ít nhất một</strong> trong hai điều kiện sau đúng:</p>

<ul>
	<li>Người dùng giành được <strong>bất kỳ</strong> huy chương nào trong <strong>ít nhất ba cuộc thi liên tiếp</strong>.</li>
	<li>Người dùng giành huy chương <strong>vàng</strong> trong <strong>ít nhất ba cuộc thi khác nhau</strong> (không nhất thiết liên tiếp).</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Contests:
+------------+------------+--------------+--------------+
| contest_id | gold_medal | silver_medal | bronze_medal |
+------------+------------+--------------+--------------+
| 190        | 1          | 5            | 2            |
| 191        | 2          | 3            | 5            |
| 192        | 5          | 2            | 3            |
| 193        | 1          | 3            | 5            |
| 194        | 4          | 5            | 2            |
| 195        | 4          | 2            | 1            |
| 196        | 1          | 5            | 2            |
+------------+------------+--------------+--------------+
Bảng Users:
+---------+--------------------+-------+
| user_id | mail               | name  |
+---------+--------------------+-------+
| 1       | sarah@leetcode.com | Sarah |
| 2       | bob@leetcode.com   | Bob   |
| 3       | alice@leetcode.com | Alice |
| 4       | hercy@leetcode.com | Hercy |
| 5       | quarz@leetcode.com | Quarz |
+---------+--------------------+-------+
<strong>Đầu ra:</strong>
+-------+--------------------+
| name  | mail               |
+-------+--------------------+
| Sarah | sarah@leetcode.com |
| Bob   | bob@leetcode.com   |
| Alice | alice@leetcode.com |
| Quarz | quarz@leetcode.com |
+-------+--------------------+
<strong>Giải thích:</strong>
Sarah giành 3 huy chương vàng (190, 193 và 196), nên ta đưa cô ấy vào bảng kết quả.
Bob giành một huy chương trong 3 cuộc thi liên tiếp (190, 191 và 192), nên ta đưa anh ấy vào bảng kết quả.
    - Lưu ý rằng anh ấy cũng giành một huy chương trong 3 cuộc thi liên tiếp khác (194, 195 và 196).
Alice giành một huy chương trong 3 cuộc thi liên tiếp (191, 192 và 193), nên ta đưa cô ấy vào bảng kết quả.
Quarz giành một huy chương trong 5 cuộc thi liên tiếp (190, 191, 192, 193 và 194), nên ta đưa họ vào bảng kết quả.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Nếu điều kiện đầu tiên đổi thành "bất kỳ huy chương nào trong <code>n</code><strong> hoặc nhiều hơn</strong> cuộc thi liên tiếp" thì bạn sẽ thay đổi lời giải thế nào để tìm các ứng viên phỏng vấn? Hãy coi <code>n</code> là tham số của một stored procedure.</li>
	<li>Một số người dùng có thể không tham gia mọi cuộc thi nhưng vẫn thể hiện tốt trong những cuộc thi họ tham gia. Bạn sẽ thay đổi câu lệnh thế nào để chỉ xét các cuộc thi mà người dùng <strong>đã tham gia</strong>? Giả sử danh sách người dùng đăng ký cho mỗi cuộc thi được cung cấp trong một bảng khác.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một ứng viên phải giành bất kỳ huy chương nào trong ba cuộc thi liên tiếp hoặc giành ít nhất ba huy chương vàng. Huy chương nằm trong ba cột, nên không thể dùng một thao tác lọc trực tiếp trên bảng gốc để biểu diễn đồng thời hai điều kiện.
>
> Chuyển gold, silver và bronze từ ba cột thành các dòng $(contest\_id,user\_id,type)$. Số huy chương vàng được đếm bằng $COUNT$ theo nhóm. Các cuộc thi liên tiếp được tìm bằng cách sắp xếp từng người dùng theo contest id rồi trừ số thứ tự dòng; hiệu không đổi đánh dấu một khối liên tiếp. Những người có khối dài ít nhất $3$, hoặc có ít nhất ba huy chương vàng, được nối lại với Users để lấy name và mail.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT contest_id, gold_medal AS user_id, 1 AS type
        FROM Contests
        UNION
        SELECT contest_id, silver_medal AS user_id, 2 AS type
        FROM Contests
        UNION
        SELECT contest_id, bronze_medal AS user_id, 3 AS type
        FROM Contests
    ),
    T AS (
        SELECT
            user_id,
            (
                contest_id - ROW_NUMBER() OVER (
                    PARTITION BY user_id
                    ORDER BY contest_id
                )
            ) AS diff
        FROM S
    ),
    P AS (
        SELECT user_id
        FROM S
        WHERE type = 1
        GROUP BY user_id
        HAVING COUNT(1) >= 3
        UNION
        SELECT DISTINCT user_id
        FROM T
        GROUP BY user_id, diff
        HAVING COUNT(1) >= 3
    )
SELECT name, mail
FROM
    P AS p
    LEFT JOIN Users AS u ON p.user_id = u.user_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
