---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1693. Daily Leads and Partners](https://leetcode.com/problems/daily-leads-and-partners)

[中文文档](/solution/1600-1699/1693.Daily%20Leads%20and%20Partners/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>DailySales</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| date_id     | date    |
| make_name   | varchar |
| lead_id     | int     |
| partner_id  | int     |
+-------------+---------+
Bảng không có khóa chính (cột chứa các giá trị duy nhất), nên có thể chứa các dòng trùng lặp.
Bảng này chứa ngày, tên sản phẩm đã bán và ID của lead và partner mà sản phẩm được bán cho.
Tên chỉ gồm các chữ cái tiếng Anh viết thường.
</pre>

<p>&nbsp;</p>

<p>Với mỗi <code>date_id</code> và <code>make_name</code>, hãy tìm số lượng <code>lead_id</code> <strong>khác nhau</strong> và số lượng <code>partner_id</code> <strong>khác nhau</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
DailySales table:
+-----------+-----------+---------+------------+
| date_id   | make_name | lead_id | partner_id |
+-----------+-----------+---------+------------+
| 2020-12-8 | toyota    | 0       | 1          |
| 2020-12-8 | toyota    | 1       | 0          |
| 2020-12-8 | toyota    | 1       | 2          |
| 2020-12-7 | toyota    | 0       | 2          |
| 2020-12-7 | toyota    | 0       | 1          |
| 2020-12-8 | honda     | 1       | 2          |
| 2020-12-8 | honda     | 2       | 1          |
| 2020-12-7 | honda     | 0       | 1          |
| 2020-12-7 | honda     | 1       | 2          |
| 2020-12-7 | honda     | 2       | 1          |
+-----------+-----------+---------+------------+
<strong>Đầu ra:</strong>
+-----------+-----------+--------------+-----------------+
| date_id   | make_name | unique_leads | unique_partners |
+-----------+-----------+--------------+-----------------+
| 2020-12-8 | toyota    | 2            | 3               |
| 2020-12-7 | toyota    | 1            | 2               |
| 2020-12-8 | honda     | 2            | 2               |
| 2020-12-7 | honda     | 3            | 2               |
+-----------+-----------+--------------+-----------------+
<strong>Giải thích:</strong>
Vào ngày 2020-12-8, toyota có leads = [0, 1] và partners = [0, 1, 2], còn honda có leads = [1, 2] và partners = [1, 2].
Vào ngày 2020-12-7, toyota có leads = [0] và partners = [1, 2], còn honda có leads = [0, 1, 2] và partners = [1, 2].
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By + Đếm Distinct

<!-- thinking:start -->

> **Tư duy**
>
> Đếm leads và partners khác nhau theo ngày và thương hiệu: $\texttt{GROUP BY date\_id, make\_name}$ và dùng $\texttt{COUNT}(\texttt{DISTINCT})$ trên hai cột.

<!-- thinking:end -->

Ta có thể sử dụng câu lệnh `GROUP BY` để nhóm dữ liệu theo các trường `date_id` và `make_name`, sau đó dùng hàm `COUNT(DISTINCT)` để đếm số giá trị khác nhau của `lead_id` và `partner_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    date_id,
    make_name,
    COUNT(DISTINCT lead_id) AS unique_leads,
    COUNT(DISTINCT partner_id) AS unique_partners
FROM DailySales
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
