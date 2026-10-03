---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2394. Employees With Deductions 🔒](https://leetcode.com/problems/employees-with-deductions)

[中文文档](/solution/2300-2399/2394.Employees%20With%20Deductions/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employees</code></p>

<pre>
+--------------+------+
| Tên cột      | Kiểu |
+--------------+------+
| employee_id  | int  |
| needed_hours | int  |
+--------------+------+
employee_id là cột có giá trị duy nhất trong bảng này.
Mỗi dòng chứa ID của một nhân viên và số giờ tối thiểu họ cần làm để nhận lương.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Logs</code></p>

<pre>
+-------------+----------+
| Tên cột     | Kiểu     |
+-------------+----------+
| employee_id | int      |
| in_time     | datetime |
| out_time    | datetime |
+-------------+----------+
(employee_id, in_time, out_time) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi dòng trong bảng này ghi lại các mốc thời gian của một nhân viên. in_time là thời điểm nhân viên bắt đầu làm việc, còn out_time là thời điểm kết thúc.
Tất cả thời gian đều thuộc tháng 10 năm 2022. out_time có thể rơi vào ngày sau in_time, nghĩa là nhân viên đã làm việc qua nửa đêm.
</pre>

<p>&nbsp;</p>

<p>Trong một công ty, mỗi nhân viên phải làm đủ một số giờ nhất định mỗi tháng. Nhân viên làm việc theo các phiên. Tổng số giờ làm việc được tính bằng tổng số phút đã làm trong tất cả các phiên. Số phút của mỗi phiên được làm tròn lên.</p>

<ul>
	<li>Ví dụ, nếu nhân viên làm việc trong một phiên được <code>51</code> phút và <code>2</code> giây, thời gian được tính là <code>52</code> phút.</li>
</ul>

<p>Hãy viết lời giải để báo cáo ID của những nhân viên bị trừ lương. Nói cách khác, hãy báo cáo ID của những nhân viên không làm đủ số giờ yêu cầu.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Employees:
+-------------+--------------+
| employee_id | needed_hours |
+-------------+--------------+
| 1           | 20           |
| 2           | 12           |
| 3           | 2            |
+-------------+--------------+
Bảng Logs:
+-------------+---------------------+---------------------+
| employee_id | in_time             | out_time            |
+-------------+---------------------+---------------------+
| 1           | 2022-10-01 09:00:00 | 2022-10-01 17:00:00 |
| 1           | 2022-10-06 09:05:04 | 2022-10-06 17:09:03 |
| 1           | 2022-10-12 23:00:00 | 2022-10-13 03:00:01 |
| 2           | 2022-10-29 12:00:00 | 2022-10-29 23:58:58 |
+-------------+---------------------+---------------------+
<strong>Đầu ra:</strong>
+-------------+
| employee_id |
+-------------+
| 2           |
| 3           |
+-------------+
<strong>Giải thích:</strong>
Nhân viên 1:
 - Đã làm việc trong ba phiên:
    - Vào ngày 2022-10-01, họ làm việc 8 giờ.
    - Vào ngày 2022-10-06, họ làm việc 8 giờ 4 phút.
    - Vào ngày 2022-10-12, họ làm việc 4 giờ 1 phút. Lưu ý rằng họ đã làm việc qua nửa đêm.
 - Nhân viên 1 đã làm tổng cộng 20 giờ 5 phút trong các phiên và không bị trừ lương.
Nhân viên 2:
 - Đã làm việc trong một phiên:
    - Vào ngày 2022-10-29, họ làm việc 11 giờ 59 phút.
 - Nhân viên 2 không làm đủ số giờ yêu cầu và sẽ bị trừ lương.
Nhân viên 3:
 - Không làm việc trong phiên nào.
 - Nhân viên 3 không làm đủ số giờ yêu cầu và sẽ bị trừ lương.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tổng số giờ được tính sau khi thời lượng của từng phiên được làm tròn lên theo phút. Liệt kê những nhân viên có tổng thời gian làm việc nhỏ hơn $needed\_hours$, bao gồm cả những người không có log.
>
> $TIMESTAMPDIFF$ tính số giây, $CEILING$ làm tròn lên thành số phút, sau đó chia cho $60$. Dùng LEFT JOIN với bảng nhân viên và xem tổng thời gian bị thiếu là $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            employee_id,
            SUM(ceiling(TIMESTAMPDIFF(second, in_time, out_time) / 60)) / 60 AS tot
        FROM Logs
        GROUP BY employee_id
    )
SELECT employee_id
FROM
    Employees
    LEFT JOIN T USING (employee_id)
WHERE IFNULL(tot, 0) < needed_hours;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
