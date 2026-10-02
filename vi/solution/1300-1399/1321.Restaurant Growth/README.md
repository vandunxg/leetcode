---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1321. Restaurant Growth](https://leetcode.com/problems/restaurant-growth)

[中文文档](/solution/1300-1399/1321.Restaurant%20Growth/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Customer</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| visited_on    | date    |
| amount        | int     |
+---------------+---------+
Trong SQL, (customer_id, visited_on) là khóa chính của bảng này.
Bảng này chứa dữ liệu về các giao dịch của khách hàng tại nhà hàng.
visited_on là ngày khách hàng có ID (customer_id) đến nhà hàng.
amount là tổng số tiền khách hàng đã trả.
</pre>

<p>&nbsp;</p>

<p>Bạn là chủ nhà hàng và muốn phân tích khả năng mở rộng hoạt động (đảm bảo mỗi ngày có ít nhất một khách hàng).</p>

<p>Tính moving average số tiền khách hàng đã trả trong cửa sổ 7 ngày (ngày hiện tại và 6 ngày trước đó). <code>average_amount</code> cần được <strong>làm tròn đến hai chữ số thập phân</strong>.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>visited_on</code> <strong>theo thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Customer:
+-------------+--------------+--------------+-------------+
| customer_id | name         | visited_on   | amount      |
+-------------+--------------+--------------+-------------+
| 1           | Jhon         | 2019-01-01   | 100         |
| 2           | Daniel       | 2019-01-02   | 110         |
| 3           | Jade         | 2019-01-03   | 120         |
| 4           | Khaled       | 2019-01-04   | 130         |
| 5           | Winston      | 2019-01-05   | 110         | 
| 6           | Elvis        | 2019-01-06   | 140         | 
| 7           | Anna         | 2019-01-07   | 150         |
| 8           | Maria        | 2019-01-08   | 80          |
| 9           | Jaze         | 2019-01-09   | 110         | 
| 1           | Jhon         | 2019-01-10   | 130         | 
| 3           | Jade         | 2019-01-10   | 150         | 
+-------------+--------------+--------------+-------------+
<strong>Đầu ra:</strong> 
+--------------+--------------+----------------+
| visited_on   | amount       | average_amount |
+--------------+--------------+----------------+
| 2019-01-07   | 860          | 122.86         |
| 2019-01-08   | 840          | 120            |
| 2019-01-09   | 840          | 120            |
| 2019-01-10   | 1000         | 142.86         |
+--------------+--------------+----------------+
<strong>Giải thích:</strong> 
Moving average thứ nhất, từ 2019-01-01 đến 2019-01-07, có average_amount bằng (100 + 110 + 120 + 130 + 110 + 140 + 150)/7 = 122.86
Moving average thứ hai, từ 2019-01-02 đến 2019-01-08, có average_amount bằng (110 + 120 + 130 + 110 + 140 + 150 + 80)/7 = 120
Moving average thứ ba, từ 2019-01-03 đến 2019-01-09, có average_amount bằng (120 + 130 + 110 + 140 + 150 + 80 + 110)/7 = 120
Moving average thứ tư, từ 2019-01-04 đến 2019-01-10, có average_amount bằng (130 + 110 + 140 + 150 + 80 + 110 + 130 + 150)/7 = 142.86
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi ngày, cần tính tổng và giá trị trung bình mỗi ngày trong cửa sổ đủ $7$ ngày kết thúc tại ngày đó, tính cả hai đầu mút. Gom amount theo ngày, rồi dùng window sum $\mathrm{ROWS}\,6\,\mathrm{PRECEDING}$ trên các ngày đã sắp xếp; bỏ sáu hạng đầu để thu được mọi moving average đủ bảy ngày.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            visited_on,
            SUM(amount) OVER (
                ORDER BY visited_on
                ROWS 6 PRECEDING
            ) AS amount,
            RANK() OVER (
                ORDER BY visited_on
                ROWS 6 PRECEDING
            ) AS rk
        FROM
            (
                SELECT visited_on, SUM(amount) AS amount
                FROM Customer
                GROUP BY visited_on
            ) AS tt
    )
SELECT visited_on, amount, ROUND(amount / 7, 2) AS average_amount
FROM t
WHERE rk > 6;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Không nhất thiết phải dùng window function. Join từng ngày riêng biệt $a$ với các dòng chi tiết $b$ có khoảng cách ngày thuộc $[0,6]$, đồng thời chỉ lấy $a$ cách ngày nhỏ nhất ít nhất sáu ngày; cách này tạo ra các tổng đủ bảy ngày tương tự mà không cần cú pháp window.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    a.visited_on,
    SUM(b.amount) AS amount,
    ROUND(SUM(b.amount) / 7, 2) AS average_amount
FROM
    (SELECT DISTINCT visited_on FROM customer) AS a
    JOIN customer AS b ON DATEDIFF(a.visited_on, b.visited_on) BETWEEN 0 AND 6
WHERE a.visited_on >= (SELECT MIN(visited_on) FROM customer) + 6
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
