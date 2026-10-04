---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3156. Employee Task Duration and Concurrent Tasks 🔒](https://leetcode.com/problems/employee-task-duration-and-concurrent-tasks)

[中文文档](/solution/3100-3199/3156.Employee%20Task%20Duration%20and%20Concurrent%20Tasks/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Tasks</code></p>

<pre>
+---------------+----------+
| Column Name   | Type     |
+---------------+----------+
| task_id       | int      |
| employee_id   | int      |
| start_time    | datetime |
| end_time      | datetime |
+---------------+----------+
(task_id, employee_id) là khóa chính của bảng này.
Mỗi hàng trong bảng này chứa mã định danh tác vụ, mã định danh nhân viên, thời điểm bắt đầu và thời điểm kết thúc của từng tác vụ.
</pre>

<p>Hãy viết lời giải để tìm <strong>tổng thời lượng</strong> tác vụ của <strong>mỗi</strong> nhân viên và <strong>số lượng tác vụ đồng thời tối đa</strong> mà một nhân viên đã xử lý tại <strong>bất kỳ thời điểm nào</strong>. Tổng thời lượng phải được <strong>làm tròn xuống</strong> đến số <strong>giờ trọn vẹn</strong> gần nhất.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em>&nbsp;<code>employee_id</code><strong> <em>thứ tự tăng dần</em></strong><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Tasks:</p>

<pre class="example-io">
+---------+-------------+---------------------+---------------------+
| task_id | employee_id | start_time          | end_time            |
+---------+-------------+---------------------+---------------------+
| 1       | 1001        | 2023-05-01 08:00:00 | 2023-05-01 09:00:00 |
| 2       | 1001        | 2023-05-01 08:30:00 | 2023-05-01 10:30:00 |
| 3       | 1001        | 2023-05-01 11:00:00 | 2023-05-01 12:00:00 |
| 7       | 1001        | 2023-05-01 13:00:00 | 2023-05-01 15:30:00 |
| 4       | 1002        | 2023-05-01 09:00:00 | 2023-05-01 10:00:00 |
| 5       | 1002        | 2023-05-01 09:30:00 | 2023-05-01 11:30:00 |
| 6       | 1003        | 2023-05-01 14:00:00 | 2023-05-01 16:00:00 |
+---------+-------------+---------------------+---------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+------------------+----------------------+
| employee_id | total_task_hours | max_concurrent_tasks |
+-------------+------------------+----------------------+
| 1001        | 6                | 2                    |
| 1002        | 2                | 2                    |
| 1003        | 2                | 1                    |
+-------------+------------------+----------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đối với nhân viên có ID 1001:
	<ul>
		<li>Tác vụ 1 và tác vụ 2 chồng lấn từ 08:30 đến 09:00 (30 phút).</li>
		<li>Tác vụ 7 có thời lượng 150 phút (2 giờ 30 phút).</li>
		<li>Tổng thời gian tác vụ: 60 (tác vụ 1) + 120 (tác vụ 2) + 60 (tác vụ 3) + 150 (tác vụ 7) - 30 (thời gian chồng lấn) = 360 phút = 6 giờ.</li>
		<li>Số tác vụ đồng thời tối đa: 2 (trong khoảng thời gian chồng lấn).</li>
	</ul>
	</li>
	<li>Đối với nhân viên có ID 1002:
	<ul>
		<li>Tác vụ 4 và tác vụ 5 chồng lấn từ 09:30 đến 10:00 (30 phút).</li>
		<li>Tổng thời gian tác vụ: 60 (tác vụ 4) + 120 (tác vụ 5) - 30 (thời gian chồng lấn) = 150 phút = 2 giờ 30 phút.</li>
		<li>Tổng số giờ tác vụ (làm tròn xuống): 2 giờ.</li>
		<li>Số tác vụ đồng thời tối đa: 2 (trong khoảng thời gian chồng lấn).</li>
	</ul>
	</li>
	<li>Đối với nhân viên có ID 1003:
	<ul>
		<li>Không có tác vụ nào chồng lấn.</li>
		<li>Tổng thời gian tác vụ: 120 phút = 2 giờ.</li>
		<li>Số tác vụ đồng thời tối đa: 1.</li>
	</ul>
	</li>
</ul>

<p><b>Lưu ý:</b> Bảng kết quả được sắp xếp theo employee_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gộp + Join

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhân viên cần tổng số giờ được phủ bởi các tác vụ và số tác vụ đồng thời cực đại. Kiểm tra chồng lấn theo từng cặp khá rắc rối trong SQL.
>
> Các thời điểm bắt đầu/kết thúc khác nhau của một nhân viên chia trục thời gian thành các đoạn nhỏ. Số tác vụ bao phủ một đoạn chính là số tác vụ đồng thời; tổng độ dài các đoạn là tổng số giờ không bị đếm trùng.
>
> Gộp các điểm biên, dùng `LEAD` để lấy thời điểm tiếp theo, join ngược với `Tasks` để đếm các dòng bao phủ, rồi aggregate theo nhân viên để tính tổng số giờ và số lượng lớn nhất.

<!-- thinking:end -->

Đầu tiên, chúng ta gộp `start_time` và `end_time` của mỗi `employee_id` vào một bảng mới `T`. Sau đó, sử dụng hàm `LEAD`, chúng ta tính thời điểm bắt đầu của tác vụ tiếp theo cho mỗi nhân viên. Tiếp theo, chúng ta join bảng `T` với bảng `Tasks` để tính số tác vụ đồng thời của mỗi nhân viên. Cuối cùng, chúng ta nhóm theo `employee_id` để tính tổng thời lượng tác vụ và số tác vụ đồng thời tối đa của mỗi nhân viên.

Bài toán tương tự:

- [3268. Find Overlapping Shifts II 🔒](https://github.com/doocs/leetcode/blob/main/solution/3200-3299/3268.Find%20Overlapping%20Shifts%20II/README_EN.md)

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT DISTINCT employee_id, start_time AS st
        FROM Tasks
        UNION DISTINCT
        SELECT DISTINCT employee_id, end_time AS st
        FROM Tasks
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
            INNER JOIN Tasks USING (employee_id)
        WHERE P.st >= Tasks.start_time AND P.ed <= Tasks.end_time
        GROUP BY 1, 2, 3
    )
SELECT
    employee_id,
    FLOOR(SUM(TIME_TO_SEC(TIMEDIFF(ed, st)) / 3600)) AS total_task_hours,
    MAX(concurrent_count) AS max_concurrent_tasks
FROM S
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
