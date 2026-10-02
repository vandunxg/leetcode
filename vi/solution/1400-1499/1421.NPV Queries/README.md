---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1421. NPV Queries 🔒](https://leetcode.com/problems/npv-queries)

[中文文档](/solution/1400-1499/1421.NPV%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>NPV</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| year          | int     |
| npv           | int     |
+---------------+---------+
(id, year) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng chứa thông tin về id, năm và giá trị hiện tại ròng tương ứng của mỗi hàng tồn kho.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Queries</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| year          | int     |
+---------------+---------+
(id, year) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng chứa thông tin về id và năm của mỗi truy vấn hàng tồn kho.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm <code>npv</code> của mỗi truy vấn trong bảng <code>Queries</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng NPV:
+------+--------+--------+
| id   | year   | npv    |
+------+--------+--------+
| 1    | 2018   | 100    |
| 7    | 2020   | 30     |
| 13   | 2019   | 40     |
| 1    | 2019   | 113    |
| 2    | 2008   | 121    |
| 3    | 2009   | 12     |
| 11   | 2020   | 99     |
| 7    | 2019   | 0      |
+------+--------+--------+
Bảng Queries:
+------+--------+
| id   | year   |
+------+--------+
| 1    | 2019   |
| 2    | 2008   |
| 3    | 2009   |
| 7    | 2018   |
| 7    | 2019   |
| 7    | 2020   |
| 13   | 2019   |
+------+--------+
<strong>Đầu ra:</strong>
+------+--------+--------+
| id   | year   | npv    |
+------+--------+--------+
| 1    | 2019   | 113    |
| 2    | 2008   | 121    |
| 3    | 2009   | 12     |
| 7    | 2018   | 0      |
| 7    | 2019   | 0      |
| 7    | 2020   | 30     |
| 13   | 2019   | 40     |
+------+--------+--------+
<strong>Giải thích:</strong>
Giá trị npv của (7, 2018) không có trong bảng NPV, nên ta xem giá trị đó là 0.
Giá trị npv của tất cả truy vấn còn lại đều có thể tìm thấy trong bảng NPV.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một truy vấn $(id,year)$ có thể không xuất hiện trong `NPV` và khi đó cần trả về $0$. `inner join` sẽ loại bỏ các dòng này, vì vậy ta `left join` `Queries` với `NPV` theo cả hai khóa.
>
> `IFNULL(npv, 0)` điền các giá trị bị thiếu mà vẫn giữ lại mọi dòng truy vấn.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT q.*, IFNULL(npv, 0) AS npv
FROM
    Queries AS q
    LEFT JOIN NPV AS n USING (id, year);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
