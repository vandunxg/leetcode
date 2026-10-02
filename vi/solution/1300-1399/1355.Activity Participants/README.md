---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1355. Activity Participants 🔒](https://leetcode.com/problems/activity-participants)

[中文文档](/solution/1300-1399/1355.Activity%20Participants/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Friends</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
| activity      | varchar |
+---------------+---------+
id là ID của người bạn và là khóa chính của bảng này trong SQL.
name là tên của người bạn.
activity là tên hoạt động mà người bạn tham gia.
</pre>

<p>&nbsp;</p>

<p>Table: <code>Activities</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
Trong SQL, id là khóa chính của bảng này.
name là tên hoạt động.
</pre>

<p>&nbsp;</p>

<p>Tìm tên các hoạt động có số người tham gia không phải là lớn nhất cũng không phải là nhỏ nhất.</p>

<p>Mỗi hoạt động trong bảng <code>Activities</code> đều có ít nhất một người trong bảng Friends tham gia.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Friends:
+------+--------------+---------------+
| id   | name         | activity      |
+------+--------------+---------------+
| 1    | Jonathan D.  | Eating        |
| 2    | Jade W.      | Singing       |
| 3    | Victor J.    | Singing       |
| 4    | Elvis Q.     | Eating        |
| 5    | Daniel A.    | Eating        |
| 6    | Bob B.       | Horse Riding  |
+------+--------------+---------------+
Bảng Activities:
+------+--------------+
| id   | name         |
+------+--------------+
| 1    | Eating       |
| 2    | Singing      |
| 3    | Horse Riding |
+------+--------------+
<strong>Đầu ra:</strong> 
+--------------+
| activity     |
+--------------+
| Singing      |
+--------------+
<strong>Giải thích:</strong> 
Hoạt động Eating có 3 người bạn tham gia, là số lượng lớn nhất (Jonathan D. , Elvis Q. và Daniel A.).
Hoạt động Horse Riding có 1 người bạn tham gia, là số lượng nhỏ nhất (Bob B.).
Hoạt động Singing có 2 người bạn tham gia (Victor J. và Jade W.).
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm các hoạt động có số người tham gia không phải lớn nhất cũng không phải nhỏ nhất. Đếm số người bạn tham gia từng hoạt động, rồi giữ lại tên các hoạt động có số lượng nằm nghiêm ngặt giữa giá trị nhỏ nhất và lớn nhất toàn cục.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT activity, COUNT(1) AS cnt
        FROM Friends
        GROUP BY activity
    )
SELECT activity
FROM t
WHERE cnt > (SELECT MIN(cnt) FROM t) AND cnt < (SELECT MAX(cnt) FROM t);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
