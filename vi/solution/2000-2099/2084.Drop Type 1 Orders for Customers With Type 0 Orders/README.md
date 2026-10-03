---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2084. Drop Type 1 Orders for Customers With Type 0 Orders 🔒](https://leetcode.com/problems/drop-type-1-orders-for-customers-with-type-0-orders)

[中文文档](/solution/2000-2099/2084.Drop%20Type%201%20Orders%20for%20Customers%20With%20Type%200%20Orders/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Orders</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| customer_id | int  |
| order_type  | int  |
+-------------+------+
order_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này cho biết ID của một đơn hàng, ID của khách hàng đã đặt đơn hàng đó và loại đơn hàng.
Đơn hàng có thể thuộc loại 0 hoặc loại 1.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo tất cả đơn hàng theo các tiêu chí sau:</p>

<ul>
	<li>Nếu một khách hàng có <strong>ít nhất một</strong> đơn hàng loại <code>0</code>, <strong>không</strong> báo cáo bất kỳ đơn hàng loại <code>1</code> nào của khách hàng đó.</li>
	<li>Nếu không, báo cáo tất cả đơn hàng của khách hàng đó.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Orders:
+----------+-------------+------------+
| order_id | customer_id | order_type |
+----------+-------------+------------+
| 1        | 1           | 0          |
| 2        | 1           | 0          |
| 11       | 2           | 0          |
| 12       | 2           | 1          |
| 21       | 3           | 1          |
| 22       | 3           | 0          |
| 31       | 4           | 1          |
| 32       | 4           | 1          |
+----------+-------------+------------+
<strong>Đầu ra:</strong>
+----------+-------------+------------+
| order_id | customer_id | order_type |
+----------+-------------+------------+
| 31       | 4           | 1          |
| 32       | 4           | 1          |
| 1        | 1           | 0          |
| 2        | 1           | 0          |
| 11       | 2           | 0          |
| 22       | 3           | 0          |
+----------+-------------+------------+
<strong>Giải thích:</strong>
Khách hàng 1 có hai đơn hàng loại 0. Ta trả về cả hai đơn hàng.
Khách hàng 2 có một đơn hàng loại 0 và một đơn hàng loại 1. Ta chỉ trả về đơn hàng loại 0.
Khách hàng 3 có một đơn hàng loại 0 và một đơn hàng loại 1. Ta chỉ trả về đơn hàng loại 0.
Khách hàng 4 có hai đơn hàng loại 1. Ta trả về cả hai đơn hàng.

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Những khách hàng có đơn hàng loại 0 phải loại bỏ tất cả các dòng loại 1; các khách hàng còn lại giữ nguyên đơn hàng của họ. Ta tính tập các khách hàng có đơn hàng loại 0, sau đó giữ lại một dòng nếu dòng đó thuộc loại 0 hoặc khách hàng của dòng đó không nằm trong tập trên.
>
> Một CTE kết hợp với `NOT EXISTS` giúp biểu diễn điều kiện lọc này.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT DISTINCT customer_id
        FROM Orders
        WHERE order_type = 0
    )
SELECT *
FROM Orders AS o
WHERE order_type = 0 OR NOT EXISTS (SELECT 1 FROM T AS t WHERE t.customer_id = o.customer_id);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng subquery. Tự nối các loại khác nhau của cùng một khách hàng: một dòng loại 1 khớp với dòng loại 0 thì phải bị loại bỏ. Giữ lại các dòng không có dòng đối tác khác loại, hoặc có dòng đối tác loại 1 (khi đó bản thân dòng này là loại 0), rồi loại bỏ các dòng trùng.
>
> Có thể đạt được kết quả tương tự bằng `LEFT JOIN`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT DISTINCT
    a.order_id,
    a.customer_id,
    a.order_type
FROM
    Orders AS a
    LEFT JOIN Orders AS b ON a.customer_id = b.customer_id AND a.order_type != b.order_type
WHERE b.order_type IS NULL OR b.order_type = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
