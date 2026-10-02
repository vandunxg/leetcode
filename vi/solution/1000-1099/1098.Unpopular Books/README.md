---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1098. Unpopular Books 🔒](https://leetcode.com/problems/unpopular-books)

[中文文档](/solution/1000-1099/1098.Unpopular%20Books/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Books</code></p>

<pre>
+----------------+---------+
| Tên cột        | Kiểu    |
+----------------+---------+
| book_id        | int     |
| name           | varchar |
| available_from | date    |
+----------------+---------+
book_id là khóa chính (cột có giá trị duy nhất) của bảng này.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Orders</code></p>

<pre>
+----------------+---------+
| Tên cột        | Kiểu    |
+----------------+---------+
| order_id       | int     |
| book_id        | int     |
| quantity       | int     |
| dispatch_date  | date    |
+----------------+---------+
order_id là khóa chính (cột có giá trị duy nhất) của bảng này.
book_id là khóa ngoại (cột tham chiếu) đến bảng Books.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm các <strong>cuốn sách</strong> đã bán <strong>ít hơn </strong><code>10</code> bản trong năm vừa qua, không tính những sách mới được mở bán chưa đủ một tháng tính đến hôm nay. <strong>Giả sử hôm nay là ngày </strong><code>2019-06-23</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Books:
+---------+--------------------+----------------+
| book_id | name               | available_from |
+---------+--------------------+----------------+
| 1       | &quot;Kalila And Demna&quot; | 2010-01-01     |
| 2       | &quot;28 Letters&quot;       | 2012-05-12     |
| 3       | &quot;The Hobbit&quot;       | 2019-06-10     |
| 4       | &quot;13 Reasons Why&quot;   | 2019-06-01     |
| 5       | &quot;The Hunger Games&quot; | 2008-09-21     |
+---------+--------------------+----------------+
Bảng Orders:
+----------+---------+----------+---------------+
| order_id | book_id | quantity | dispatch_date |
+----------+---------+----------+---------------+
| 1        | 1       | 2        | 2018-07-26    |
| 2        | 1       | 1        | 2018-11-05    |
| 3        | 3       | 8        | 2019-06-11    |
| 4        | 4       | 6        | 2019-06-05    |
| 5        | 4       | 5        | 2019-06-20    |
| 6        | 5       | 9        | 2009-02-02    |
| 7        | 5       | 8        | 2010-04-13    |
+----------+---------+----------+---------------+
<strong>Đầu ra:</strong> 
+-----------+--------------------+
| book_id   | name               |
+-----------+--------------------+
| 1         | &quot;Kalila And Demna&quot; |
| 2         | &quot;28 Letters&quot;       |
| 5         | &quot;The Hunger Games&quot; |
+-----------+--------------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Loại các sách mới mở bán chưa đủ một tháng, rồi giữ lại những sách bán được ít hơn $10$ bản trong năm vừa qua. Sách không có đơn hàng được tính là bán 0 bản, vì vậy cần dùng left join.
>
> Lọc theo `available_from < '2019-05-23'`, left join với `Orders`, rồi chỉ cộng `quantity` khi `dispatch_date` nằm trong khoảng một năm cần xét.
>
> `HAVING` giữ lại các sách có tổng số bản bán dưới $10$. Trong `SUM(IF(...))`, những đơn hàng nằm ngoài khoảng thời gian này đóng góp giá trị $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT book_id, name
FROM
    Books
    LEFT JOIN Orders USING (book_id)
WHERE available_from < '2019-05-23'
GROUP BY 1
HAVING SUM(IF(dispatch_date >= '2018-06-23', quantity, 0)) < 10;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
