---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1729. Find Followers Count](https://leetcode.com/problems/find-followers-count)

[中文文档](/solution/1700-1799/1729.Find%20Followers%20Count/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Followers</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| follower_id | int  |
+-------------+------+
(user_id, follower_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng này chứa ID của một người dùng và một follower trong ứng dụng mạng xã hội, trong đó follower theo dõi người dùng đó.</pre>

<p>&nbsp;</p>

<p>Viết lời giải trả về số lượng follower của mỗi người dùng.</p>

<p>Trả về bảng kết quả được sắp xếp theo thứ tự tăng dần của <code>user_id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong>
Followers table:
+---------+-------------+
| user_id | follower_id |
+---------+-------------+
| 0       | 1           |
| 1       | 0           |
| 2       | 0           |
| 2       | 1           |
+---------+-------------+
<strong>Output:</strong>
+---------+----------------+
| user_id | followers_count|
+---------+----------------+
| 0       | 1              |
| 1       | 1              |
| 2       | 2              |
+---------+----------------+
<strong>Giải thích:</strong>
Các follower của 0 là {1}
Các follower của 1 là {0}
Các follower của 2 là {0,1}
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi dòng là một cạnh theo dõi. Ta cần số follower của từng người dùng, được sắp xếp theo $\textit{user\_id}$.
>
> Nhóm $\textit{Followers}$ theo $\textit{user\_id}$, dùng $\mathrm{COUNT}$ để đếm các dòng rồi sắp xếp theo cột đầu tiên.

<!-- thinking:end -->

Ta có thể trực tiếp nhóm bảng `Followers` theo `user_id` và dùng hàm `COUNT` để đếm số follower của mỗi người dùng.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT user_id, COUNT(1) AS followers_count
FROM Followers
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
