---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1741. Find Total Time Spent by Each Employee](https://leetcode.com/problems/find-total-time-spent-by-each-employee)

[中文文档](/solution/1700-1799/1741.Find%20Total%20Time%20Spent%20by%20Each%20Employee/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employees</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| emp_id      | int  |
| event_day   | date |
| in_time     | int  |
| out_time    | int  |
+-------------+------+
(emp_id, event_day, in_time) is the primary key (combinations of columns with unique values) of this table.
Bảng này ghi lại thời điểm nhân viên vào và rời văn phòng.
event_day là ngày sự kiện xảy ra, in_time là phút nhân viên vào văn phòng, còn out_time là phút họ rời văn phòng.
in_time and out_time are between 1 and 1440.
It is guaranteed that no two events on the same day intersect in time, and in_time &lt; out_time.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tính tổng thời gian <strong>tính bằng phút</strong> mỗi nhân viên ở văn phòng trong từng ngày. Lưu ý rằng trong một ngày, một nhân viên có thể vào và rời văn phòng nhiều lần. Thời gian ở văn phòng trong một lần vào là <code>out_time - in_time</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Employees:
+--------+------------+---------+----------+
| emp_id | event_day  | in_time | out_time |
+--------+------------+---------+----------+
| 1      | 2020-11-28 | 4       | 32       |
| 1      | 2020-11-28 | 55      | 200      |
| 1      | 2020-12-03 | 1       | 42       |
| 2      | 2020-11-28 | 3       | 33       |
| 2      | 2020-12-09 | 47      | 74       |
+--------+------------+---------+----------+
<strong>Đầu ra:</strong>
+------------+--------+------------+
| day        | emp_id | total_time |
+------------+--------+------------+
| 2020-11-28 | 1      | 173        |
| 2020-11-28 | 2      | 30         |
| 2020-12-03 | 1      | 41         |
| 2020-12-09 | 2      | 27         |
+------------+--------+------------+
<strong>Giải thích:</strong>
Nhân viên 1 có ba sự kiện: hai sự kiện vào ngày 2020-11-28 với tổng thời gian (32 - 4) + (200 - 55) = 173, và một sự kiện vào ngày 2020-12-03 với tổng thời gian (42 - 1) = 41.
Nhân viên 2 có hai sự kiện: một sự kiện vào ngày 2020-11-28 với tổng thời gian (33 - 3) = 30, và một sự kiện vào ngày 2020-12-09 với tổng thời gian (74 - 47) = 27.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By + Hàm Sum

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng là một cặp vào/rời. Ta cần tổng thời gian ở văn phòng của từng nhân viên trong từng ngày.
>
> Group by $\textit{event\_day}$ và $\textit{emp\_id}$, rồi tính tổng $\textit{out\_time}-\textit{in\_time}$.

<!-- thinking:end -->

Trước hết, ta group by `emp_id` và `event_day`, sau đó tính tổng thời gian cho từng nhóm. Tổng thời gian bằng tổng các hiệu giữa `out_time` và `in_time` của mỗi bản ghi trong nhóm.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT event_day AS day, emp_id, SUM(out_time - in_time) AS total_time
FROM Employees
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
