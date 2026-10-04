---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3166. Calculate Parking Fees and Duration 🔒](https://leetcode.com/problems/calculate-parking-fees-and-duration)

[中文文档](/solution/3100-3199/3166.Calculate%20Parking%20Fees%20and%20Duration/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>ParkingTransactions</code></p>

<pre>
+--------------+-----------+
| Column Name  | Type      |
+--------------+-----------+
| lot_id       | int       |
| car_id       | int       |
| entry_time   | datetime  |
| exit_time    | datetime  |
| fee_paid     | decimal   |
+--------------+-----------+
(lot_id, car_id, entry_time) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi dòng trong bảng này chứa ID của bãi đỗ xe, ID của xe, thời gian vào và ra, cùng phí đã trả cho thời gian đỗ xe.
</pre>

<p>Viết lời giải để tìm <strong>tổng phí đỗ xe</strong> mà mỗi xe đã trả <strong>trên tất cả các bãi đỗ xe</strong>, cùng với <strong>phí trung bình mỗi giờ</strong> (làm tròn đến <code>2</code> chữ số thập phân) mà <strong>từng</strong> xe đã trả. Ngoài ra, hãy tìm <strong>bãi đỗ xe</strong> nơi mỗi xe đã dành <strong>tổng thời gian</strong> nhiều nhất.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo </em><code>car_id</code><em><b> </b>theo<b> thứ tự tăng dần </b></em><em>.</em></p>

<p><strong>Lưu ý:</strong> Các test case được tạo sao cho một xe không thể ở nhiều bãi đỗ xe cùng lúc.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng ParkingTransactions:</p>

<pre class="example-io">
+--------+--------+---------------------+---------------------+----------+
| lot_id | car_id | entry_time          | exit_time           | fee_paid |
+--------+--------+---------------------+---------------------+----------+
| 1      | 1001   | 2023-06-01 08:00:00 | 2023-06-01 10:30:00 | 5.00     |
| 1      | 1001   | 2023-06-02 11:00:00 | 2023-06-02 12:45:00 | 3.00     |
| 2      | 1001   | 2023-06-01 10:45:00 | 2023-06-01 12:00:00 | 6.00     |
| 2      | 1002   | 2023-06-01 09:00:00 | 2023-06-01 11:30:00 | 4.00     |
| 3      | 1001   | 2023-06-03 07:00:00 | 2023-06-03 09:00:00 | 4.00     |
| 3      | 1002   | 2023-06-02 12:00:00 | 2023-06-02 14:00:00 | 2.00     |
+--------+--------+---------------------+---------------------+----------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+--------+----------------+----------------+---------------+
| car_id | total_fee_paid | avg_hourly_fee | most_time_lot |
+--------+----------------+----------------+---------------+
| 1001   | 18.00          | 2.40           | 1             |
| 1002   | 6.00           | 1.33           | 2             |
+--------+----------------+----------------+---------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Đối với xe có ID 1001:
    <ul>
        <li>Từ 2023-06-01 08:00:00 đến 2023-06-01 10:30:00 tại bãi đỗ 1: 2.5 giờ, phí 5.00</li>
        <li>Từ 2023-06-02 11:00:00 đến 2023-06-02 12:45:00 tại bãi đỗ 1: 1.75 giờ, phí 3.00</li>
        <li>Từ 2023-06-01 10:45:00 đến 2023-06-01 12:00:00 tại bãi đỗ 2: 1.25 giờ, phí 6.00</li>
        <li>Từ 2023-06-03 07:00:00 đến 2023-06-03 09:00:00 tại bãi đỗ 3: 2 giờ, phí 4.00</li>
    </ul>
    Tổng phí đã trả: 18.00, tổng số giờ: 7.5, phí trung bình mỗi giờ: 2.40, dành nhiều thời gian nhất ở bãi đỗ 1: 4.25 giờ.</li>
    <li>Đối với xe có ID 1002:
    <ul>
        <li>Từ 2023-06-01 09:00:00 đến 2023-06-01 11:30:00 tại bãi đỗ 2: 2.5 giờ, phí 4.00</li>
        <li>Từ 2023-06-02 12:00:00 đến 2023-06-02 14:00:00 tại bãi đỗ 3: 2 giờ, phí 2.00</li>
    </ul>
    Tổng phí đã trả: 6.00, tổng số giờ: 4.5, phí trung bình mỗi giờ: 1.33, dành nhiều thời gian nhất ở bãi đỗ 2: 2.5 giờ.</li>
</ul>

<p><b>Lưu ý:</b> Bảng kết quả được sắp xếp theo car_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm + Join

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi xe cần tổng phí, phí trung bình theo giờ và bãi đỗ nơi xe ở lâu nhất. Nếu chỉ group-by đơn giản, cần thêm một lượt xử lý.
>
> Cộng dồn thời lượng theo $(car\_id,lot\_id)$, rồi xếp hạng các bãi đỗ của từng xe theo thời lượng đó để đánh dấu nơi ở lâu nhất.
>
> Tổng hợp phí và số giây từ bảng gốc, left-join với bãi đỗ xếp hạng $1$, rồi chia phí cho số giờ và làm tròn đến hai chữ số thập phân.

<!-- thinking:end -->

Trước tiên, chúng ta có thể nhóm theo `car_id` và `lot_id` để tính thời gian đỗ của mỗi xe tại từng bãi đỗ. Sau đó, dùng hàm `RANK()` để xếp hạng thời gian đỗ của mỗi xe tại từng bãi đỗ, từ đó tìm bãi đỗ nơi mỗi xe có thời gian đỗ lâu nhất.

Cuối cùng, chúng ta có thể nhóm theo `car_id` để tính tổng phí đỗ xe, phí trung bình mỗi giờ và bãi đỗ có thời gian đỗ lâu nhất của từng xe.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            car_id,
            lot_id,
            SUM(TIMESTAMPDIFF(SECOND, entry_time, exit_time)) AS duration
        FROM ParkingTransactions
        GROUP BY 1, 2
    ),
    P AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY car_id
                ORDER BY duration DESC
            ) AS rk
        FROM T
    )
SELECT
    t1.car_id,
    SUM(fee_paid) AS total_fee_paid,
    ROUND(
        SUM(fee_paid) / (SUM(TIMESTAMPDIFF(SECOND, entry_time, exit_time)) / 3600),
        2
    ) AS avg_hourly_fee,
    t2.lot_id AS most_time_lot
FROM
    ParkingTransactions AS t1
    LEFT JOIN P AS t2 ON t1.car_id = t2.car_id AND t2.rk = 1
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
