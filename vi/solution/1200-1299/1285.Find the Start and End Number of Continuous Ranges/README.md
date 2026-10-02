---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1285. Find the Start and End Number of Continuous Ranges 🔒](https://leetcode.com/problems/find-the-start-and-end-number-of-continuous-ranges)

[中文文档](/solution/1200-1299/1285.Find%20the%20Start%20and%20End%20Number%20of%20Continuous%20Ranges/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Logs</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| log_id        | int     |
+---------------+---------+
`log_id` là cột có giá trị duy nhất trong bảng này.
Mỗi hàng trong bảng này chứa ID của một log.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải tìm số bắt đầu và số kết thúc của các đoạn liên tiếp trong bảng <code>Logs</code>.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>start_id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Logs:
+------------+
| log_id     |
+------------+
| 1          |
| 2          |
| 3          |
| 7          |
| 8          |
| 10         |
+------------+
<strong>Đầu ra:</strong> 
+------------+--------------+
| start_id   | end_id       |
+------------+--------------+
| 1          | 3            |
| 7          | 8            |
| 10         | 10           |
+------------+--------------+
<strong>Giải thích:</strong> 
Bảng kết quả cần chứa tất cả các đoạn trong bảng Logs.
Các số từ 1 đến 3 có trong bảng.
Các số từ 4 đến 6 không có trong bảng.
Các số từ 7 đến 8 có trong bảng.
Số 9 không có trong bảng.
Số 10 có trong bảng.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: GROUP BY + hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Hai log liên tiếp chênh nhau $1$. Ta gán $0$ nếu hiệu với hàng trước là $1$, ngược lại gán $1$; sau đó tính prefix sum để đưa mỗi đoạn liên tiếp vào cùng một nhóm. $MIN$/$MAX$ của $log\_id$ theo mã nhóm là hai đầu mút của đoạn.

<!-- thinking:end -->

Ta cần nhóm các dãy log liên tiếp vào cùng một nhóm, sau đó tổng hợp từng nhóm để lấy log đầu và cuối.

Có hai cách để thực hiện việc nhóm:

1. Tính hiệu giữa mỗi log và log trước đó. Nếu hiệu bằng $1$, hai log liên tiếp nhau và ta đặt $delta$ bằng $0$; ngược lại, đặt $delta$ bằng $1$. Sau đó tính prefix sum của $delta$ để tạo mã nhóm cho mỗi hàng.
2. Tính hiệu giữa log hiện tại và số thứ tự hàng để thu được mã nhóm cho mỗi hàng.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            log_id,
            SUM(delta) OVER (ORDER BY log_id) AS pid
        FROM
            (
                SELECT
                    log_id,
                    IF((log_id - LAG(log_id) OVER (ORDER BY log_id)) = 1, 0, 1) AS delta
                FROM Logs
            ) AS t
    )
SELECT MIN(log_id) AS start_id, MAX(log_id) AS end_id
FROM T
GROUP BY pid;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 mã hóa các khoảng đứt quãng rồi tính prefix sum. Hiệu giữa $log\_id$ và số thứ tự hàng không đổi trong một đoạn liên tiếp, nên hiệu này có thể làm khóa nhóm; nhờ đó ta bỏ được $LAG$ và $delta$ có điều kiện.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            log_id,
            log_id - ROW_NUMBER() OVER (ORDER BY log_id) AS pid
        FROM Logs
    )
SELECT MIN(log_id) AS start_id, MAX(log_id) AS end_id
FROM T
GROUP BY pid;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
