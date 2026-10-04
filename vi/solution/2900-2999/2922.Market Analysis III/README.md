---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2922. Market Analysis III 🔒](https://leetcode.com/problems/market-analysis-iii)

[中文文档](/solution/2900-2999/2922.Market%20Analysis%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Users</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| seller_id      | int     |
| join_date      | date    |
| favorite_brand | varchar |
+----------------+---------+
seller_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa seller id, ngày tham gia và thương hiệu yêu thích của người bán.
</pre>

<p>Bảng: <code>Items</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| item_id       | int     |
| item_brand    | varchar |
+---------------+---------+
item_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa item id và thương hiệu của item.</pre>

<p>Bảng: <code>Orders</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| item_id       | int     |
| seller_id     | int     |
+---------------+---------+
order_id là cột có các giá trị duy nhất trong bảng này.
item_id là khóa ngoại tham chiếu đến bảng Items.
seller_id là khóa ngoại tham chiếu đến bảng Users.
Bảng này chứa order id, ngày đặt hàng, item id và seller id.</pre>

<p>Hãy viết lời giải để tìm <strong>người bán hàng đầu</strong>, là người đã bán số lượng <strong>item duy nhất</strong> lớn nhất có <strong>thương hiệu khác</strong> với thương hiệu yêu thích của họ. Nếu có nhiều người bán có cùng số lượng lớn nhất, hãy trả về tất cả họ.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>seller_id</code> <em>theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Users table:
+-----------+------------+----------------+
| seller_id | join_date  | favorite_brand |
+-----------+------------+----------------+
| 1         | 2019-01-01 | Lenovo         |
| 2         | 2019-02-09 | Samsung        |
| 3         | 2019-01-19 | LG             |
+-----------+------------+----------------+
Orders table:
+----------+------------+---------+-----------+
| order_id | order_date | item_id | seller_id |
+----------+------------+---------+-----------+
| 1        | 2019-08-01 | 4       | 2         |
| 2        | 2019-08-02 | 2       | 3         |
| 3        | 2019-08-03 | 3       | 3         |
| 4        | 2019-08-04 | 1       | 2         |
| 5        | 2019-08-04 | 4       | 2         |
+----------+------------+---------+-----------+
Items table:
+---------+------------+
| item_id | item_brand |
+---------+------------+
| 1       | Samsung    |
| 2       | Lenovo     |
| 3       | LG         |
| 4       | HP         |
+---------+------------+
<strong>Đầu ra:</strong>
+-----------+-----------+
| seller_id | num_items |
+-----------+-----------+
| 2         | 1         |
| 3         | 1         |
+-----------+-----------+
<strong>Giải thích:</strong>
- Người dùng có seller_id bằng 2 đã bán ba item, nhưng chỉ có hai item không phải thương hiệu yêu thích. Ta chỉ tính 1 item duy nhất vì hai item này giống hệt nhau.
- Người dùng có seller_id bằng 3 đã bán hai item, nhưng chỉ có một item không phải thương hiệu yêu thích. Ta chỉ tính item không yêu thích đó.
Vì seller_id 2 và 3 có cùng số lượng là một item, cả hai đều được hiển thị trong kết quả.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phép nối bằng + Nhóm + Truy vấn con

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi người bán, ta cần số lượng item phân biệt có thương hiệu khác với thương hiệu yêu thích của người đó, sau đó tìm những người bán đạt giá trị lớn nhất. Nối orders, users và items giúp liên kết ba bảng; chỉ còn bộ lọc $item_brand \neq favorite_brand$ và việc nhóm theo $seller_id$.
>
> $COUNT(DISTINCT item_id)$ cho ta $num_items$. So sánh với truy vấn con $MAX(num_items)$ giúp tránh trộn phép tổng hợp với bộ lọc trong cùng một tầng. Cuối cùng, sắp xếp theo seller id.

<!-- thinking:end -->

Ta có thể dùng phép nối bằng để kết nối bảng `Orders` và bảng `Users` theo `seller_id`, sau đó kết nối với `Items` theo `item_id`, rồi lọc các bản ghi mà `item_brand` không bằng `favorite_brand`. Tiếp theo, nhóm theo `seller_id` và đếm số lượng `item_id` tương ứng với mỗi `seller_id`. Cuối cùng, dùng một truy vấn con để tìm `seller_id` có nhiều `item_id` nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT seller_id, COUNT(DISTINCT item_id) AS num_items
        FROM
            Orders
            JOIN Users USING (seller_id)
            JOIN Items USING (item_id)
        WHERE item_brand != favorite_brand
        GROUP BY 1
    )
SELECT seller_id, num_items
FROM T
WHERE num_items = (SELECT MAX(num_items) FROM T)
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
