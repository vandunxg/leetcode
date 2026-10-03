---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2082. The Number of Rich Customers 🔒](https://leetcode.com/problems/the-number-of-rich-customers)

[中文文档](/solution/2000-2099/2082.The%20Number%20of%20Rich%20Customers/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Store</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| bill_id     | int  |
| customer_id | int  |
| amount      | int  |
+-------------+------+
bill_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về số tiền của một hóa đơn và khách hàng tương ứng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết một lời giải để báo cáo số lượng khách hàng có <strong>ít nhất một</strong> hóa đơn có số tiền <strong>lớn hơn</strong> <code>500</code>.</p>

<p>Kết quả được hiển thị theo định dạng trong ví dụ dưới đây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Store:
+---------+-------------+--------+
| bill_id | customer_id | amount |
+---------+-------------+--------+
| 6       | 1           | 549    |
| 8       | 1           | 834    |
| 4       | 2           | 394    |
| 11      | 3           | 657    |
| 13      | 3           | 257    |
+---------+-------------+--------+
<strong>Đầu ra:</strong>
+------------+
| rich_count |
+------------+
| 2          |
+------------+
<strong>Giải thích:</strong>
Khách hàng 1 có hai hóa đơn với số tiền lớn hơn 500.
Khách hàng 2 không có hóa đơn nào có số tiền lớn hơn 500.
Khách hàng 3 có một hóa đơn với số tiền lớn hơn 500.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một khách hàng được xem là giàu nếu có bất kỳ hóa đơn nào vượt quá $500$. Các khách hàng trùng lặp không được tính hai lần, vì vậy dùng `COUNT(DISTINCT customer_id)` với `WHERE amount > 500`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    COUNT(DISTINCT customer_id) AS rich_count
FROM Store
WHERE amount > 500;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
