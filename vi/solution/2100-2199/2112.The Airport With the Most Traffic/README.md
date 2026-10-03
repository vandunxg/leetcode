---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2112. The Airport With the Most Traffic 🔒](https://leetcode.com/problems/the-airport-with-the-most-traffic)

[中文文档](/solution/2100-2199/2112.The%20Airport%20With%20the%20Most%20Traffic/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Flights</code></p>

<pre>
+-------------------+------+
| Column Name       | Type |
+-------------------+------+
| departure_airport | int  |
| arrival_airport   | int  |
| flights_count     | int  |
+-------------------+------+
(departure_airport, arrival_airport) là cột khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này cho biết có flights_count chuyến bay khởi hành từ departure_airport và đến arrival_airport.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo ID của sân bay có <strong>nhiều lưu lượng nhất</strong>. Sân bay có nhiều lưu lượng nhất là sân bay có tổng số chuyến bay khởi hành từ hoặc đến sân bay đó lớn nhất. Nếu có nhiều sân bay cùng có tổng lưu lượng lớn nhất, hãy báo cáo tất cả các sân bay đó.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Flights:
+-------------------+-----------------+---------------+
| departure_airport | arrival_airport | flights_count |
+-------------------+-----------------+---------------+
| 1                 | 2               | 4             |
| 2                 | 1               | 5             |
| 2                 | 4               | 5             |
+-------------------+-----------------+---------------+
<strong>Đầu ra:</strong>
+------------+
| airport_id |
+------------+
| 2          |
+------------+
<strong>Giải thích:</strong>
Sân bay 1 tham gia 9 chuyến bay (4 chuyến khởi hành, 5 chuyến đến).
Sân bay 2 tham gia 14 chuyến bay (10 chuyến khởi hành, 4 chuyến đến).
Sân bay 4 tham gia 5 chuyến bay (5 chuyến đến).
Sân bay có nhiều lưu lượng nhất là sân bay 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Flights:
+-------------------+-----------------+---------------+
| departure_airport | arrival_airport | flights_count |
+-------------------+-----------------+---------------+
| 1                 | 2               | 4             |
| 2                 | 1               | 5             |
| 3                 | 4               | 5             |
| 4                 | 3               | 4             |
| 5                 | 6               | 7             |
+-------------------+-----------------+---------------+
<strong>Đầu ra:</strong>
+------------+
| airport_id |
+------------+
| 1          |
| 2          |
| 3          |
| 4          |
+------------+
<strong>Giải thích:</strong>
Sân bay 1 tham gia 9 chuyến bay (4 chuyến khởi hành, 5 chuyến đến).
Sân bay 2 tham gia 9 chuyến bay (5 chuyến khởi hành, 4 chuyến đến).
Sân bay 3 tham gia 9 chuyến bay (5 chuyến khởi hành, 4 chuyến đến).
Sân bay 4 tham gia 9 chuyến bay (4 chuyến khởi hành, 5 chuyến đến).
Sân bay 5 tham gia 7 chuyến bay (7 chuyến khởi hành).
Sân bay 6 tham gia 7 chuyến bay (7 chuyến đến).
Các sân bay có nhiều lưu lượng nhất là 1, 2, 3 và 4.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chuyến bay góp vào lưu lượng của cả hai đầu, nhưng bảng chỉ lưu một hàng có hướng. Nếu chỉ nhóm theo $\textit{departure\_airport}$, ta sẽ bỏ sót lưu lượng chuyến bay đến.
>
> Gộp $\textit{Flights}$ với một bản sao trong đó hai sân bay được hoán đổi, sau đó tính tổng theo sân bay khởi hành để lưu lượng đến và đi của mỗi sân bay nằm trong cùng một cột.
>
> Lọc các hàng đã tổng hợp có số lượng bằng giá trị lớn nhất trên toàn bộ bảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT * FROM Flights
        UNION
        SELECT arrival_airport, departure_airport, flights_count FROM Flights
    ),
    P AS (
        SELECT departure_airport, SUM(flights_count) AS cnt
        FROM T
        GROUP BY 1
    )
SELECT departure_airport AS airport_id
FROM P
WHERE cnt = (SELECT MAX(cnt) FROM P);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
