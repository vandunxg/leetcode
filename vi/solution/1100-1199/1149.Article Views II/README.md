---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1149. Article Views II 🔒](https://leetcode.com/problems/article-views-ii)

[中文文档](/solution/1100-1199/1149.Article%20Views%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Views</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Mỗi hàng trong bảng cho biết một người xem đã xem một bài viết (do một tác giả viết) vào một ngày nào đó. 
Lưu ý rằng nếu author_id và viewer_id bằng nhau thì đó là cùng một người.</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm tất cả những người đã xem nhiều bài viết khác nhau trong cùng một ngày.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>id</code> tăng dần.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Views:
+------------+-----------+-----------+------------+
| article_id | author_id | viewer_id | view_date  |
+------------+-----------+-----------+------------+
| 1          | 3         | 5         | 2019-08-01 |
| 3          | 4         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |
+------------+-----------+-----------+------------+
<strong>Đầu ra:</strong> 
+------+
| id   |
+------+
| 5    |
| 6    |
+------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm người xem đã đọc ít nhất hai bài viết khác nhau trong cùng một ngày. Dùng `GROUP BY viewer_id, view_date` với `HAVING COUNT(DISTINCT article_id) > 1`, sau đó lấy `DISTINCT` viewer để mỗi người chỉ xuất hiện một lần dù thỏa điều kiện ở nhiều ngày.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DISTINCT viewer_id AS id
FROM Views
GROUP BY viewer_id, view_date
HAVING COUNT(DISTINCT article_id) > 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
