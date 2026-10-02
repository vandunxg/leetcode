---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1645. Hopper Company Queries II 🔒](https://leetcode.com/problems/hopper-company-queries-ii)

[中文文档](/solution/1600-1699/1645.Hopper%20Company%20Queries%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Drivers</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| driver_id   | int     |
| join_date   | date    |
+-------------+---------+
driver_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa ID của tài xế và ngày họ gia nhập công ty Hopper.
</pre>

<p>&nbsp;</p>

<p>Table: <code>Rides</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| ride_id      | int     |
| user_id      | int     |
| requested_at | date    |
+--------------+---------+
ride_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa ID chuyến đi, ID người dùng đã yêu cầu chuyến đi và ngày họ yêu cầu.
Có thể có một số yêu cầu chuyến đi trong bảng này chưa được chấp nhận.
</pre>

<p>&nbsp;</p>

<p>Table: <code>AcceptedRides</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| ride_id       | int     |
| driver_id     | int     |
| ride_distance | int     |
| ride_duration | int     |
+---------------+---------+
ride_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này chứa một số thông tin về một chuyến đi đã được chấp nhận.
Đảm bảo mỗi chuyến đi đã chấp nhận đều tồn tại trong bảng Rides.
</pre>

<p>&nbsp;</p>

<p>Viết truy vấn báo cáo <strong>tỷ lệ</strong> tài xế đang làm việc (<code>working_percentage</code>) cho từng tháng của <strong>2020</strong>, trong đó:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1645.Hopper%20Company%20Queries%20II/images/codecogseqn.png" style="width: 800px; height: 36px;" />
<p><strong>Lưu ý</strong> rằng nếu số tài xế sẵn sàng trong một tháng bằng 0, ta coi <code>working_percentage</code> là <code>0</code>.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>month</code> theo thứ tự <strong>tăng dần</strong>, trong đó <code>month</code> là số của tháng (tháng Một là <code>1</code>, tháng Hai là <code>2</code>, v.v.). Làm tròn <code>working_percentage</code> đến <strong>2 chữ số thập phân</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Drivers table:
+-----------+------------+
| driver_id | join_date  |
+-----------+------------+
| 10        | 2019-12-10 |
| 8         | 2020-1-13  |
| 5         | 2020-2-16  |
| 7         | 2020-3-8   |
| 4         | 2020-5-17  |
| 1         | 2020-10-24 |
| 6         | 2021-1-5   |
+-----------+------------+
Rides table:
+---------+---------+--------------+
| ride_id | user_id | requested_at |
+---------+---------+--------------+
| 6       | 75      | 2019-12-9    |
| 1       | 54      | 2020-2-9     |
| 10      | 63      | 2020-3-4     |
| 19      | 39      | 2020-4-6     |
| 3       | 41      | 2020-6-3     |
| 13      | 52      | 2020-6-22    |
| 7       | 69      | 2020-7-16    |
| 17      | 70      | 2020-8-25    |
| 20      | 81      | 2020-11-2    |
| 5       | 57      | 2020-11-9    |
| 2       | 42      | 2020-12-9    |
| 11      | 68      | 2021-1-11    |
| 15      | 32      | 2021-1-17    |
| 12      | 11      | 2021-1-19    |
| 14      | 18      | 2021-1-27    |
+---------+---------+--------------+
AcceptedRides table:
+---------+-----------+---------------+---------------+
| ride_id | driver_id | ride_distance | ride_duration |
+---------+-----------+---------------+---------------+
| 10      | 10        | 63            | 38            |
| 13      | 10        | 73            | 96            |
| 7       | 8         | 100           | 28            |
| 17      | 7         | 119           | 68            |
| 20      | 1         | 121           | 92            |
| 5       | 7         | 42            | 101           |
| 2       | 4         | 6             | 38            |
| 11      | 8         | 37            | 43            |
| 15      | 8         | 108           | 82            |
| 12      | 8         | 38            | 34            |
| 14      | 1         | 90            | 74            |
+---------+-----------+---------------+---------------+
<strong>Output:</strong> 
+-------+--------------------+
| month | working_percentage |
+-------+--------------------+
| 1     | 0.00               |
| 2     | 0.00               |
| 3     | 25.00              |
| 4     | 0.00               |
| 5     | 0.00               |
| 6     | 20.00              |
| 7     | 20.00              |
| 8     | 20.00              |
| 9     | 0.00               |
| 10    | 0.00               |
| 11    | 33.33              |
| 12    | 16.67              |
+-------+--------------------+
<strong>Explanation:</strong> 
Đến cuối tháng Một --&gt; có hai tài xế đang hoạt động (10, 8) và không có chuyến đi nào được chấp nhận. Tỷ lệ là 0%.
Đến cuối tháng Hai --&gt; có ba tài xế đang hoạt động (10, 8, 5) và không có chuyến đi nào được chấp nhận. Tỷ lệ là 0%.
Đến cuối tháng Ba --&gt; có bốn tài xế đang hoạt động (10, 8, 5, 7) và một chuyến đi được tài xế (10) chấp nhận. Tỷ lệ là (1 / 4) * 100 = 25%.
Đến cuối tháng Tư --&gt; có bốn tài xế đang hoạt động (10, 8, 5, 7) và không có chuyến đi nào được chấp nhận. Tỷ lệ là 0%.
Đến cuối tháng Năm --&gt; có năm tài xế đang hoạt động (10, 8, 5, 7, 4) và không có chuyến đi nào được chấp nhận. Tỷ lệ là 0%.
Đến cuối tháng Sáu --&gt; có năm tài xế đang hoạt động (10, 8, 5, 7, 4) và một chuyến đi được tài xế (10) chấp nhận. Tỷ lệ là (1 / 5) * 100 = 20%.
Đến cuối tháng Bảy --&gt; có năm tài xế đang hoạt động (10, 8, 5, 7, 4) và một chuyến đi được tài xế (8) chấp nhận. Tỷ lệ là (1 / 5) * 100 = 20%.
Đến cuối tháng Tám --&gt; có năm tài xế đang hoạt động (10, 8, 5, 7, 4) và một chuyến đi được tài xế (7) chấp nhận. Tỷ lệ là (1 / 5) * 100 = 20%.
Đến cuối tháng Chín --&gt; có năm tài xế đang hoạt động (10, 8, 5, 7, 4) và không có chuyến đi nào được chấp nhận. Tỷ lệ là 0%.
Đến cuối tháng Mười --&gt; có sáu tài xế đang hoạt động (10, 8, 5, 7, 4, 1) và không có chuyến đi nào được chấp nhận. Tỷ lệ là 0%.
Đến cuối tháng Mười Một --&gt; có sáu tài xế đang hoạt động (10, 8, 5, 7, 4, 1) và hai chuyến đi được hai tài xế <strong>khác nhau</strong> (1, 7) chấp nhận. Tỷ lệ là (2 / 6) * 100 = 33.33%.
Đến cuối tháng Mười Hai --&gt; có sáu tài xế đang hoạt động (10, 8, 5, 7, 4, 1) và một chuyến đi được tài xế (4) chấp nhận. Tỷ lệ là (1 / 6) * 100 = 16.67%.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tỷ lệ làm việc là số tài xế khác nhau đã nhận chuyến đi chia cho số tài xế đã được tuyển đến tháng đó. Mọi tháng đều phải xuất hiện, và tháng không có tài xế có giá trị $0$.
>
> Một danh sách tháng đệ quy được left join với drivers, sau đó left join với các chuyến đi đã nhận trong $2020$; $\texttt{COUNT}(\texttt{DISTINCT})$ tạo ra tỷ lệ.
>
> Phép join cũng yêu cầu $\texttt{join\_date} \le \texttt{requested\_at}$ để không tính tài xế trước khi họ được tuyển.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH RECURSIVE
    Month AS (
        SELECT 1 AS month
        UNION
        SELECT month + 1
        FROM Month
        WHERE month < 12
    ),
    S AS (
        SELECT month, driver_id, join_date
        FROM
            Month AS m
            LEFT JOIN Drivers AS d
                ON YEAR(d.join_date) < 2020
                OR (YEAR(d.join_date) = 2020 AND MONTH(d.join_date) <= month)
    ),
    T AS (
        SELECT driver_id, requested_at
        FROM
            Rides
            JOIN AcceptedRides USING (ride_id)
        WHERE YEAR(requested_at) = 2020
    )
SELECT
    month,
    IFNULL(
        ROUND(COUNT(DISTINCT t.driver_id) * 100 / COUNT(DISTINCT s.driver_id), 2),
        0
    ) AS working_percentage
FROM
    S AS s
    LEFT JOIN T AS t
        ON s.driver_id = t.driver_id
        AND s.join_date <= t.requested_at
        AND s.month = MONTH(t.requested_at)
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
