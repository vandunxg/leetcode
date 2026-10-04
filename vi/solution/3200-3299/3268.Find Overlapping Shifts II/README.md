---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3268. Find Overlapping Shifts II 🔒](https://leetcode.com/problems/find-overlapping-shifts-ii)

[Tài liệu tiếng Trung](/solution/3200-3299/3268.Find%20Overlapping%20Shifts%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>EmployeeShifts</code></p>

<pre>
+------------------+----------+
| Column Name      | Type     |
+------------------+----------+
| employee_id      | int      |
| start_time       | datetime |
| end_time         | datetime |
+------------------+----------+
(employee_id, start_time) is the unique key for this table.
This table contains information about the shifts worked by employees, including the start time, and end time.
</pre>

<p>Viết lời giải để phân tích các ca làm việc chồng lấn của từng nhân viên. Hai ca được xem là chồng lấn nếu chúng diễn ra trong <strong>cùng một ngày</strong> và <code>end_time</code> của một ca <strong>muộn hơn</strong> <code>start_time</code> của ca còn lại.</p>

<p>Với <strong>mỗi nhân viên</strong>, hãy tính:</p>

<ol>
	<li><strong>Số lượng lớn nhất</strong> các ca làm việc <strong>chồng lấn</strong> tại <strong>bất kỳ thời điểm nào</strong>.</li>
	<li><strong>Tổng thời lượng</strong> chồng lấn tính bằng phút.</li>
</ol>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>employee_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>EmployeeShifts</code>:</p>

<pre class="example-io">
+-------------+---------------------+---------------------+
| employee_id | start_time          | end_time            |
+-------------+---------------------+---------------------+
| 1           | 2023-10-01 09:00:00 | 2023-10-01 17:00:00 |
| 1           | 2023-10-01 15:00:00 | 2023-10-01 23:00:00 |
| 1           | 2023-10-01 16:00:00 | 2023-10-02 00:00:00 |
| 2           | 2023-10-01 09:00:00 | 2023-10-01 17:00:00 |
| 2           | 2023-10-01 11:00:00 | 2023-10-01 19:00:00 |
| 3           | 2023-10-01 09:00:00 | 2023-10-01 17:00:00 |
+-------------+---------------------+---------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+---------------------------+------------------------+
| employee_id | max_overlapping_shifts    | total_overlap_duration |
+-------------+---------------------------+------------------------+
| 1           | 3                         | 600                    |
| 2           | 2                         | 360                    |
| 3           | 1                         | 0                      |
+-------------+---------------------------+------------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Nhân viên 1 có 3 ca:
	<ul>
		<li>2023-10-01 09:00:00 đến 2023-10-01 17:00:00</li>
		<li>2023-10-01 15:00:00 đến 2023-10-01 23:00:00</li>
		<li>2023-10-01 16:00:00 đến 2023-10-02 00:00:00</li>
	</ul>
	Số ca chồng lấn lớn nhất là 3 (từ 16:00 đến 17:00). Tổng thời lượng chồng lấn là: - 2 giờ (15:00-17:00) giữa ca thứ 1 và thứ 2 - 1 giờ (16:00-17:00) giữa ca thứ 1 và thứ 3 - 7 giờ (16:00-23:00) giữa ca thứ 2 và thứ 3 Tổng cộng: 10 giờ = 600 phút</li>
	<li>Nhân viên 2 có 2 ca:
	<ul>
		<li>2023-10-01 09:00:00 đến 2023-10-01 17:00:00</li>
		<li>2023-10-01 11:00:00 đến 2023-10-01 19:00:00</li>
	</ul>
	Số ca chồng lấn lớn nhất là 2. Tổng thời lượng chồng lấn là 6 giờ (11:00-17:00) = 360 phút.</li>
	<li>Nhân viên 3 chỉ có 1 ca nên không có ca chồng lấn.</li>
</ul>

<p>Bảng kết quả gồm employee_id, số ca chồng lấn đồng thời lớn nhất và tổng thời lượng chồng lấn tính bằng phút của từng nhân viên, được sắp xếp theo employee_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Merge + Join

<!-- thinking:start -->

> **Tư duy**
>
> Ngoài số lượng cặp, ta cần tìm số ca đồng thời lớn nhất và tổng số phút chồng lấn. Self-join cho ta thời lượng theo từng cặp; còn số ca đồng thời cần chia timeline tại mọi thời điểm bắt đầu và kết thúc.
>
> Các endpoint khác nhau của mỗi nhân viên tạo thành các khoảng nguyên tử bằng `LEAD`, sau đó join ngược để đếm số ca bao phủ; số phút theo từng cặp vẫn dùng self-join. Với mỗi nhân viên, ta lấy số ca đồng thời lớn nhất và tổng thời lượng.

<!-- thinking:end -->

Ta có thể gộp tất cả `start_time` và `end_time` của mỗi `employee_id` rồi lưu vào bảng `T`. Sau đó, sử dụng hàm `LEAD`, ta tính khoảng thời gian tiếp theo cho mỗi `employee_id` và lưu vào bảng `P`.

Tiếp theo, ta có thể join bảng `P` với bảng `EmployeeShifts` để tính `concurrent_count` cho mỗi `employee_id`, biểu thị số khoảng thời gian chồng lấn. Kết quả này được lưu vào bảng `S`.

Cuối cùng, ta có thể self-join bảng `EmployeeShifts` để tính `total_overlap_duration` cho mỗi `employee_id`, biểu thị tổng thời gian chồng lấn, rồi lưu vào bảng `U`.

Sau cùng, ta join các bảng `S` và `U` để tính `max_overlapping_shifts` và `total_overlap_duration` cho mỗi `employee_id`.

Các bài tương tự:

- [3156. Employee Task Duration and Concurrent Tasks 🔒](https://github.com/doocs/leetcode/blob/main/solution/3100-3199/3156.Employee%20Task%20Duration%20and%20Concurrent%20Tasks/README_EN.md)
- [3262. Find Overlapping Shifts 🔒](https://github.com/doocs/leetcode/blob/main/solution/3200-3299/3262.Find%20Overlapping%20Shifts/README_EN.md)

<!-- tabs:start -->

#### MySQL

```sql
WITH
    T AS (
        SELECT DISTINCT employee_id, start_time AS st
        FROM EmployeeShifts
        UNION DISTINCT
        SELECT DISTINCT employee_id, end_time AS st
        FROM EmployeeShifts
    ),
    P AS (
        SELECT
            *,
            LEAD(st) OVER (
                PARTITION BY employee_id
                ORDER BY st
            ) AS ed
        FROM T
    ),
    S AS (
        SELECT
            P.*,
            COUNT(1) AS concurrent_count
        FROM
            P
            INNER JOIN EmployeeShifts USING (employee_id)
        WHERE P.st >= EmployeeShifts.start_time AND P.ed <= EmployeeShifts.end_time
        GROUP BY 1, 2, 3
    ),
    U AS (
        SELECT
            t1.employee_id,
            SUM(
                TIMESTAMPDIFF(MINUTE, t2.start_time, LEAST(t1.end_time, t2.end_time))
            ) total_overlap_duration
        FROM
            EmployeeShifts t1
            JOIN EmployeeShifts t2
                ON t1.employee_id = t2.employee_id
                AND t1.start_time < t2.start_time
                AND t1.end_time > t2.start_time
        GROUP BY 1
    )
SELECT
    employee_id,
    MAX(concurrent_count) max_overlapping_shifts,
    IFNULL(AVG(total_overlap_duration), 0) total_overlap_duration
FROM
    S
    LEFT JOIN U USING (employee_id)
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
