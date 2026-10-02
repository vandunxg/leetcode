---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [586. Customer Placing the Largest Number of Orders](https://leetcode.com/problems/customer-placing-the-largest-number-of-orders)

[中文文档](/solution/0500-0599/0586.Customer%20Placing%20the%20Largest%20Number%20of%20Orders/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Orders</code></p>

<pre>
+-----------------+----------+
| Column Name     | Type     |
+-----------------+----------+
| order_number    | int      |
| customer_number | int      |
+-----------------+----------+
order_number là khóa chính (cột có giá trị duy nhất) của bảng này.
Bảng này chứa thông tin về ID đơn hàng và ID khách hàng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm <code>customer_number</code> của khách hàng đã đặt <strong>nhiều đơn hàng nhất</strong>.</p>

<p>Các test case được tạo sao cho <strong>chỉ có duy nhất một khách hàng</strong> đặt nhiều đơn hơn tất cả khách hàng khác.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Orders:
+--------------+-----------------+
| order_number | customer_number |
+--------------+-----------------+
| 1            | 1               |
| 2            | 2               |
| 3            | 3               |
| 4            | 3               |
+--------------+-----------------+
<strong>Đầu ra:</strong> 
+-----------------+
| customer_number |
+-----------------+
| 3               |
+-----------------+
<strong>Giải thích:</strong> 
Khách hàng số 3 có hai đơn hàng, nhiều hơn khách hàng số 1 hoặc số 2 vì mỗi người chỉ có một đơn hàng. 
Vì vậy, kết quả là customer_number 3.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu có nhiều khách hàng cùng đặt số lượng đơn hàng lớn nhất thì bạn có thể tìm tất cả <code>customer_number</code> tương ứng không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Tìm khách hàng có nhiều đơn hàng nhất: nhóm theo khách hàng, đếm số đơn rồi lấy hàng đầu tiên.
>
> Dùng `GROUP BY customer_number` và `ORDER BY COUNT(*) DESC`, rồi giới hạn kết quả còn một hàng. Đề bài đảm bảo chỉ có một khách hàng thắng.

<!-- thinking:end -->

Ta có thể dùng `GROUP BY` để nhóm dữ liệu theo `customer_number`, sau đó sắp xếp các nhóm theo `count(1)` giảm dần. Cuối cùng, lấy `customer_number` của hàng đầu tiên làm kết quả.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    customer_number
FROM orders
GROUP BY customer_number
ORDER BY COUNT(1) DESC
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng `LIMIT 1`. Trong T-SQL, có thể lấy hàng đầu tiên sau khi đếm bằng `TOP 1`.
>
> Cách nhóm và sắp xếp không đổi, chỉ khác cú pháp giới hạn số hàng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
/* Write your T-SQL query statement below */
SELECT TOP 1
    customer_number
FROM
    orders
GROUP BY customer_number
ORDER BY COUNT(customer_number) DESC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
