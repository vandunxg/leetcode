---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [614. Second Degree Follower 🔒](https://leetcode.com/problems/second-degree-follower)

[中文文档](/solution/0600-0699/0614.Second%20Degree%20Follower/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Follow</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| followee    | varchar |
| follower    | varchar |
+-------------+---------+
(followee, follower) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng biểu thị rằng người dùng follower theo dõi người dùng followee trên mạng xã hội.
Không có người dùng nào tự theo dõi chính mình.
</pre>

<p>&nbsp;</p>

<p><strong>Follower bậc hai</strong> là người dùng thỏa mãn:</p>

<ul>
	<li>theo dõi ít nhất một người dùng, và</li>
	<li>được ít nhất một người dùng theo dõi.</li>
</ul>

<p>Hãy viết truy vấn trả về <strong>những người dùng là follower bậc hai</strong> và số người theo dõi họ.</p>

<p>Trả về bảng kết quả, <strong>sắp xếp</strong> theo <code>follower</code> <strong>theo thứ tự bảng chữ cái</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Follow:
+----------+----------+
| followee | follower |
+----------+----------+
| Alice    | Bob      |
| Bob      | Cena     |
| Bob      | Donald   |
| Donald   | Edward   |
+----------+----------+
<strong>Đầu ra:</strong> 
+----------+-----+
| follower | num |
+----------+-----+
| Bob      | 2   |
| Donald   | 1   |
+----------+-----+
<strong>Giải thích:</strong> 
Người dùng Bob có 2 người theo dõi. Bob là follower bậc hai vì anh ấy theo dõi Alice, nên được đưa vào bảng kết quả.
Người dùng Donald có 1 người theo dõi. Donald là follower bậc hai vì anh ấy theo dõi Bob, nên được đưa vào bảng kết quả.
Người dùng Alice có 1 người theo dõi. Alice không phải follower bậc hai vì cô ấy không theo dõi ai, nên không được đưa vào bảng kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Follower bậc hai vừa theo dõi người khác, vừa được người khác theo dõi. Chỉ quét bảng một lần không thể xác định cả hai mối quan hệ.
>
> Join theo điều kiện `f1.follower = f2.followee` để liệt kê những người mà họ theo dõi, sau đó dùng `COUNT(DISTINCT followee)` cho từng follower.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT f1.follower AS follower, f2.follower AS followee
        FROM
            Follow AS f1
            JOIN Follow AS f2 ON f1.follower = f2.followee
    )
SELECT follower, COUNT(DISTINCT followee) AS num
FROM T
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
