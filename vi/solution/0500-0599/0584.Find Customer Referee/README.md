---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee)

[中文文档](/solution/0500-0599/0584.Find%20Customer%20Referee/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Customer</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+
Trong SQL, id là cột khóa chính của bảng này.
Mỗi hàng trong bảng cho biết id của khách hàng, tên của họ và id của khách hàng đã giới thiệu họ.
</pre>

<p>&nbsp;</p>

<p>Tìm tên những khách hàng thuộc một trong các trường hợp sau:</p>

<ol>
	<li>Được <strong>giới thiệu bởi</strong> khách hàng bất kỳ có <code>id != 2</code>.</li>
	<li><strong>Không được</strong> khách hàng nào giới thiệu.</li>
</ol>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Customer:
+----+------+------------+
| id | name | referee_id |
+----+------+------------+
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |
+----+------+------------+
<strong>Đầu ra:</strong> 
+------+
| name |
+------+
| Will |
| Jane |
| Bill |
| Zack |
+------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lọc có điều kiện

<!-- thinking:start -->

> **Tư duy**
>
> Giữ lại những khách hàng có referee khác $2$. Trong SQL, `NULL <> 2` cho kết quả unknown, nên các hàng đó sẽ bị loại.
>
> Viết `referee_id != 2 OR referee_id IS NULL` (hoặc dùng `IFNULL`). Những khách hàng không có người giới thiệu vẫn được giữ trong kết quả.

<!-- thinking:end -->

Ta có thể lọc trực tiếp tên những khách hàng có `referee_id` khác `2`. Lưu ý rằng khách hàng có `referee_id` là `NULL` cũng cần được lọc ra.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT name
FROM Customer
WHERE IFNULL(referee_id, 0) != 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
