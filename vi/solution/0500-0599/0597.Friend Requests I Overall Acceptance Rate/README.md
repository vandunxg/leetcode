---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [597. Friend Requests I Overall Acceptance Rate 🔒](https://leetcode.com/problems/friend-requests-i-overall-acceptance-rate)

[中文文档](/solution/0500-0599/0597.Friend%20Requests%20I%20Overall%20Acceptance%20Rate/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>FriendRequest</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| sender_id      | int     |
| send_to_id     | int     |
| request_date   | date    |
+----------------+---------+
Bảng này có thể chứa các bản ghi trùng lặp (nói cách khác, bảng không có khóa chính trong SQL).
Bảng chứa ID của người gửi lời mời, ID của người nhận lời mời và ngày gửi lời mời.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>RequestAccepted</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+
Bảng này có thể chứa các bản ghi trùng lặp (nói cách khác, bảng không có khóa chính trong SQL).
Bảng chứa ID của người gửi lời mời, ID của người nhận lời mời và ngày lời mời được chấp nhận.
</pre>

<p>&nbsp;</p>

<p>Hãy tìm tỷ lệ chấp nhận lời mời tổng thể, bằng số lời mời được chấp nhận chia cho số lời mời đã gửi. Làm tròn kết quả đến 2 chữ số thập phân.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Các lời mời được chấp nhận không nhất thiết phải có trong bảng <code>friend_request</code>. Hãy đếm tổng số lời mời được chấp nhận, bất kể chúng có nằm trong danh sách lời mời ban đầu hay không, rồi chia cho số lời mời để tính tỷ lệ chấp nhận.</li>
	<li>Người gửi có thể gửi nhiều lời mời cho cùng một người nhận, và một lời mời có thể được chấp nhận nhiều lần. Trong trường hợp này, chỉ tính mỗi lời mời hoặc lượt chấp nhận bị trùng lặp một lần.</li>
	<li>Nếu không có lời mời nào, hãy trả về 0.00 cho <code>accept_rate</code>.</li>
</ul>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng FriendRequest:
+-----------+------------+--------------+
| sender_id | send_to_id | request_date |
+-----------+------------+--------------+
| 1         | 2          | 2016/06/01   |
| 1         | 3          | 2016/06/01   |
| 1         | 4          | 2016/06/01   |
| 2         | 3          | 2016/06/02   |
| 3         | 4          | 2016/06/09   |
+-----------+------------+--------------+
Bảng RequestAccepted:
+--------------+-------------+-------------+
| requester_id | accepter_id | accept_date |
+--------------+-------------+-------------+
| 1            | 2           | 2016/06/03  |
| 1            | 3           | 2016/06/08  |
| 2            | 3           | 2016/06/08  |
| 3            | 4           | 2016/06/09  |
| 3            | 4           | 2016/06/10  |
+--------------+-------------+-------------+
<strong>Đầu ra:</strong> 
+-------------+
| accept_rate |
+-------------+
| 0.8         |
+-------------+
<strong>Giải thích:</strong> 
Có 4 lời mời được chấp nhận không trùng lặp và tổng cộng có 5 lời mời. Vì vậy, tỷ lệ là 0.80.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Bạn có thể tìm tỷ lệ chấp nhận theo từng tháng không?</li>
	<li>Bạn có thể tìm tỷ lệ chấp nhận tích lũy cho từng ngày không?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tỷ lệ chấp nhận là số cặp người gửi-người nhận được chấp nhận không trùng lặp chia cho số cặp lời mời không trùng lặp; nếu không có lời mời thì tỷ lệ bằng $0$. Chỉ cần đếm số lượng phân biệt ở hai bảng.
>
> Dùng `COUNT(DISTINCT ...)` trên từng bảng, `IFNULL` để xử lý mẫu số bằng 0, rồi `ROUND` để làm tròn đến hai chữ số thập phân. Không cần join.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    ROUND(
        IFNULL(
            (
                SELECT COUNT(DISTINCT requester_id, accepter_id)
                FROM RequestAccepted
            ) / (SELECT COUNT(DISTINCT sender_id, send_to_id) FROM FriendRequest),
            0
        ),
        2
    ) AS accept_rate;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
