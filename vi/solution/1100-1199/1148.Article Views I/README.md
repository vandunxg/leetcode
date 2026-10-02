---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1148. Article Views I](https://leetcode.com/problems/article-views-i)

[中文文档](/solution/1100-1199/1148.Article%20Views%20I/README.md)

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
Bảng này không có khóa chính (cột chứa các giá trị duy nhất), nên có thể có các hàng bị trùng lặp.
Mỗi hàng cho biết một người xem đã xem bài viết do một tác giả viết vào một ngày cụ thể.
Lưu ý rằng author_id và viewer_id có giá trị bằng nhau nghĩa là cùng một người.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm tất cả tác giả đã xem ít nhất một bài viết của chính mình.</p>

<p>Trả về bảng kết quả được sắp xếp theo thứ tự tăng dần của <code>id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Bảng Views:
+------------+-----------+-----------+------------+
| article_id | author_id | viewer_id | view_date  |
+------------+-----------+-----------+------------+
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |
+------------+-----------+-----------+------------+
<strong>Output:</strong> 
+------+
| id   |
+------+
| 4    |
| 7    |
+------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lượt tự xem là các hàng có `author_id = viewer_id`. Lọc các hàng này, loại bỏ id tác giả trùng lặp bằng `DISTINCT`, rồi sắp xếp kết quả.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
