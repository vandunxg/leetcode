---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1821. Find Customers With Positive Revenue this Year 🔒](https://leetcode.com/problems/find-customers-with-positive-revenue-this-year)

[中文文档](/solution/1800-1899/1821.Find%20Customers%20With%20Positive%20Revenue%20this%20Year/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Customers</code></p>

<pre>
+--------------+------+
| Tên cột     | Kiểu |
+--------------+------+
| customer_id  | int  |
| year         | int  |
| revenue      | int  |
+--------------+------+
(customer_id, year) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng này chứa mã khách hàng và doanh thu của khách hàng trong các năm khác nhau.
Lưu ý rằng doanh thu này có thể âm.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm các khách hàng có <strong>doanh thu dương</strong> trong năm 2021.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Customers:
+-------------+------+---------+
| customer_id | year | revenue |
+-------------+------+---------+
| 1           | 2018 | 50      |
| 1           | 2021 | 30      |
| 1           | 2020 | 70      |
| 2           | 2021 | -50     |
| 3           | 2018 | 10      |
| 3           | 2016 | 50      |
| 4           | 2021 | 20      |
+-------------+------+---------+
<strong>Đầu ra:</strong>
+-------------+
| customer_id |
+-------------+
| 1           |
| 4           |
+-------------+
<strong>Giải thích:</strong>
Khách hàng 1 có doanh thu bằng 30 trong năm 2021.
Khách hàng 2 có doanh thu bằng -50 trong năm 2021.
Khách hàng 3 không có doanh thu trong năm 2021.
Khách hàng 4 có doanh thu bằng 20 trong năm 2021.
Vì vậy, chỉ khách hàng 1 và 4 có doanh thu dương trong năm 2021.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mệnh đề WHERE

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần những khách hàng có doanh thu dương trong năm $2021$. Cả $year$ và $revenue$ đều đã là các cột có sẵn, nên không cần join hay phép tổng hợp.
>
> Dùng mệnh đề $\textit{WHERE}$ để lọc theo năm và dấu của doanh thu, rồi chọn $customer\_id$ là đủ để trả lời query.

<!-- thinking:end -->

Ta có thể dùng trực tiếp mệnh đề `WHERE` để lọc những khách hàng có `year` bằng `2021` và `revenue` lớn hơn $0$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    customer_id
FROM Customers
WHERE year = '2021' AND revenue > 0;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
