---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [602. Friend Requests II Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends)

[中文文档](/solution/0600-0699/0602.Friend%20Requests%20II%20Who%20Has%20the%20Most%20Friends/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>RequestAccepted</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+
(requester_id, accepter_id) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Bảng chứa ID của người gửi lời mời, ID của người nhận lời mời và ngày lời mời được chấp nhận.
</pre>

<p>&nbsp;</p>

<p>Hãy viết truy vấn tìm người có nhiều bạn bè nhất và số lượng bạn bè của người đó.</p>

<p>Các test case được tạo sao cho chỉ có một người có nhiều bạn bè nhất.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng RequestAccepted:
+--------------+-------------+-------------+
| requester_id | accepter_id | accept_date |
+--------------+-------------+-------------+
| 1            | 2           | 2016/06/03  |
| 1            | 3           | 2016/06/08  |
| 2            | 3           | 2016/06/08  |
| 3            | 4           | 2016/06/09  |
+--------------+-------------+-------------+
<strong>Đầu ra:</strong> 
+----+-----+
| id | num |
+----+-----+
| 3  | 3   |
+----+-----+
<strong>Giải thích:</strong> 
Người có id 3 là bạn của những người 1, 2 và 4, nên có tổng cộng 3 người bạn, nhiều hơn bất kỳ ai khác.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Trong thực tế, nhiều người có thể cùng có số bạn bè lớn nhất. Bạn có thể tìm tất cả những người đó không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union All + Group By

<!-- thinking:start -->

> **Tư duy**
>
> Quan hệ bạn bè là hai chiều, nhưng mỗi lượt chấp nhận chỉ được lưu một lần. Chỉ nhóm theo người gửi hoặc chỉ theo người nhận sẽ bỏ sót phía còn lại.
>
> Dùng `UNION ALL` để thêm cả hai chiều, nhờ đó mỗi người xuất hiện ở vị trí nguồn một lần cho mỗi người bạn. Sau đó `GROUP BY` và lấy số lượng lớn nhất.

<!-- thinking:end -->

Ta có thể gộp hai cột `requester_id` và `accepter_id` thành một cột để biểu diễn quan hệ bạn bè của từng người. Sau đó, nhóm kết quả đã gộp và đếm để tìm người có nhiều bạn bè nhất cùng số lượng bạn bè của họ.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT requester_id, accepter_id FROM RequestAccepted
        UNION ALL
        SELECT accepter_id, requester_id FROM RequestAccepted
    )
SELECT requester_id AS id, COUNT(1) AS num
FROM T
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
