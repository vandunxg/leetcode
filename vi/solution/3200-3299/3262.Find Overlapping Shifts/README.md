---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3262. Find Overlapping Shifts 🔒](https://leetcode.com/problems/find-overlapping-shifts)

[中文文档](/solution/3200-3299/3262.Find%20Overlapping%20Shifts/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>EmployeeShifts</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| start_time       | time    |
| end_time         | time    |
+------------------+---------+
(employee_id, start_time) is the unique key for this table.
This table contains information about the shifts worked by employees, including the start and end times on a specific date.
</pre>

<p>Viết một lời giải để đếm số <strong>ca làm việc chồng lấn</strong> của mỗi nhân viên. Hai ca được xem là chồng lấn nếu <code>end_time</code> của một ca <strong>muộn hơn</strong> <code>start_time</code> của một ca khác.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>employee_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>EmployeeShifts</code>:</p>

<pre class="example-io">
+-------------+------------+----------+
| employee_id | start_time | end_time |
+-------------+------------+----------+
| 1           | 08:00:00   | 12:00:00 |
| 1           | 11:00:00   | 15:00:00 |
| 1           | 14:00:00   | 18:00:00 |
| 2           | 09:00:00   | 17:00:00 |
| 2           | 16:00:00   | 20:00:00 |
| 3           | 10:00:00   | 12:00:00 |
| 3           | 13:00:00   | 15:00:00 |
| 3           | 16:00:00   | 18:00:00 |
| 4           | 08:00:00   | 10:00:00 |
| 4           | 09:00:00   | 11:00:00 |
+-------------+------------+----------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+--------------------+
| employee_id | overlapping_shifts |
+-------------+--------------------+
| 1           | 2                  |
| 2           | 1                  |
| 4           | 1                  |
+-------------+--------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Nhân viên 1 có 3 ca làm việc:
	<ul>
		<li>08:00:00 đến 12:00:00</li>
		<li>11:00:00 đến 15:00:00</li>
		<li>14:00:00 đến 18:00:00</li>
	</ul>
	Ca đầu tiên chồng lấn với ca thứ hai, và ca thứ hai chồng lấn với ca thứ ba, nên có tổng cộng 2 ca chồng lấn.</li>
	<li>Nhân viên 2 có 2 ca làm việc:
	<ul>
		<li>09:00:00 đến 17:00:00</li>
		<li>16:00:00 đến 20:00:00</li>
	</ul>
	Hai ca này chồng lấn với nhau, nên có tổng cộng 1 ca chồng lấn.</li>
	<li>Nhân viên 3 có 3 ca làm việc:
	<ul>
		<li>10:00:00 đến 12:00:00</li>
		<li>13:00:00 đến 15:00:00</li>
		<li>16:00:00 đến 18:00:00</li>
	</ul>
	Không có ca nào trong số này chồng lấn, nên nhân viên 3 không xuất hiện trong kết quả.</li>
	<li>Nhân viên 4 có 2 ca làm việc:
	<ul>
		<li>08:00:00 đến 10:00:00</li>
		<li>09:00:00 đến 11:00:00</li>
	</ul>
	Hai ca này chồng lấn với nhau, nên có tổng cộng 1 ca chồng lấn.</li>
</ul>

<p>Kết quả hiển thị employee_id và số ca chồng lấn của mỗi nhân viên có ít nhất một ca chồng lấn, được sắp xếp theo employee_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Self-Join + Đếm theo nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp ca làm việc chồng lấn của cùng một nhân viên. Có thể sắp xếp theo từng nhân viên; self-join biểu diễn trực tiếp điều kiện “ca bắt đầu sớm hơn và kết thúc sau thời điểm bắt đầu của ca kia”.
>
> JOIN theo `employee_id`, với $t_1$ bắt đầu sớm hơn và kết thúc sau thời điểm bắt đầu của $t_2$, sau đó đếm theo nhân viên và loại bỏ các nhân viên có số lượng bằng 0. Mỗi cặp chồng lấn có thứ tự được đếm đúng một lần.

<!-- thinking:end -->

Trước tiên, chúng ta sử dụng self-join để nối bảng `EmployeeShifts` với chính nó. Điều kiện JOIN đảm bảo rằng chúng ta chỉ so sánh các ca thuộc cùng một nhân viên và kiểm tra xem hai ca có chồng lấn hay không.

1. `t1.start_time < t2.start_time`: Đảm bảo thời điểm bắt đầu của ca đầu tiên sớm hơn thời điểm bắt đầu của ca thứ hai.
2. `t1.end_time > t2.start_time`: Đảm bảo thời điểm kết thúc của ca đầu tiên muộn hơn thời điểm bắt đầu của ca thứ hai.

Tiếp theo, chúng ta nhóm dữ liệu theo `employee_id` và đếm số ca chồng lấn của mỗi nhân viên.

Cuối cùng, chúng ta lọc những nhân viên có số ca chồng lấn lớn hơn $0$ và sắp xếp kết quả theo `employee_id` theo thứ tự tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    t1.employee_id,
    COUNT(*) AS overlapping_shifts
FROM
    EmployeeShifts t1
    JOIN EmployeeShifts t2
        ON t1.employee_id = t2.employee_id
        AND t1.start_time < t2.start_time
        AND t1.end_time > t2.start_time
GROUP BY 1
HAVING overlapping_shifts > 0
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def find_overlapping_shifts(employee_shifts: pd.DataFrame) -> pd.DataFrame:
    merged_shifts = employee_shifts.merge(
        employee_shifts, on="employee_id", suffixes=("_t1", "_t2")
    )
    overlapping_shifts = merged_shifts[
        (merged_shifts["start_time_t1"] < merged_shifts["start_time_t2"])
        & (merged_shifts["end_time_t1"] > merged_shifts["start_time_t2"])
    ]
    result = (
        overlapping_shifts.groupby("employee_id")
        .size()
        .reset_index(name="overlapping_shifts")
    )
    result = result[result["overlapping_shifts"] > 0]
    result = result.sort_values("employee_id").reset_index(drop=True)
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
