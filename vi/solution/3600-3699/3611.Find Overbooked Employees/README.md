---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3611. Find Overbooked Employees](https://leetcode.com/problems/find-overbooked-employees)

[中文文档](/solution/3600-3699/3611.Find%20Overbooked%20Employees/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>employees</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| employee_id   | int     |
| employee_name | varchar |
| department    | varchar |
+---------------+---------+
employee_id là định danh duy nhất của bảng này.
Mỗi dòng chứa thông tin về một nhân viên và phòng ban của họ.
</pre>

<p>Bảng: <code>meetings</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| meeting_id    | int     |
| employee_id   | int     |
| meeting_date  | date    |
| meeting_type  | varchar |
| duration_hours| decimal |
+---------------+---------+
meeting_id là định danh duy nhất của bảng này.
Mỗi dòng biểu diễn một cuộc họp mà một nhân viên tham dự. meeting_type có thể là &#39;Team&#39;, &#39;Client&#39; hoặc &#39;Training&#39;.
</pre>

<p>Hãy viết lời giải để tìm những nhân viên có <strong>quá nhiều cuộc họp</strong> - những nhân viên dành hơn <code>50%</code> thời gian làm việc cho các cuộc họp trong bất kỳ tuần nào.</p>

<ul>
    <li>Giả sử một tuần làm việc tiêu chuẩn có <code>40</code><strong> giờ</strong></li>
    <li>Tính <strong>tổng số giờ họp</strong> của từng nhân viên <strong>trong mỗi tuần</strong> (<strong>Thứ Hai đến Chủ nhật</strong>)</li>
    <li>Một nhân viên có quá nhiều cuộc họp nếu số giờ họp trong tuần <code>&gt;</code> <code>20</code> giờ (<code>50%</code> của <code>40</code> giờ)</li>
    <li>Đếm số tuần mỗi nhân viên có quá nhiều cuộc họp</li>
    <li><strong>Chỉ đưa vào kết quả</strong> những nhân viên có quá nhiều cuộc họp trong <strong>ít nhất </strong><code>2</code><strong> tuần</strong></li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo số tuần có quá nhiều cuộc họp theo thứ tự <strong>giảm dần</strong>, sau đó theo tên nhân viên theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng employees:</p>

<pre class="example-io">
+-------------+----------------+-------------+
| employee_id | employee_name  | department  |
+-------------+----------------+-------------+
| 1           | Alice Johnson  | Engineering |
| 2           | Bob Smith      | Marketing   |
| 3           | Carol Davis    | Sales       |
| 4           | David Wilson   | Engineering |
| 5           | Emma Brown     | HR          |
+-------------+----------------+-------------+
</pre>

<p>Bảng meetings:</p>

<pre class="example-io">
+------------+-------------+--------------+--------------+----------------+
| meeting_id | employee_id | meeting_date | meeting_type | duration_hours |
+------------+-------------+--------------+--------------+----------------+
| 1          | 1           | 2023-06-05   | Team         | 8.0            |
| 2          | 1           | 2023-06-06   | Client       | 6.0            |
| 3          | 1           | 2023-06-07   | Training     | 7.0            |
| 4          | 1           | 2023-06-12   | Team         | 12.0           |
| 5          | 1           | 2023-06-13   | Client       | 9.0            |
| 6          | 2           | 2023-06-05   | Team         | 15.0           |
| 7          | 2           | 2023-06-06   | Client       | 8.0            |
| 8          | 2           | 2023-06-12   | Training     | 10.0           |
| 9          | 3           | 2023-06-05   | Team         | 4.0            |
| 10         | 3           | 2023-06-06   | Client       | 3.0            |
| 11         | 4           | 2023-06-05   | Team         | 25.0           |
| 12         | 4           | 2023-06-19   | Client       | 22.0           |
| 13         | 5           | 2023-06-05   | Training     | 2.0            |
+------------+-------------+--------------+--------------+----------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+----------------+-------------+---------------------+
| employee_id | employee_name  | department  | meeting_heavy_weeks |
+-------------+----------------+-------------+---------------------+
| 1           | Alice Johnson  | Engineering | 2                   |
| 4           | David Wilson   | Engineering | 2                   |
+-------------+----------------+-------------+---------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Alice Johnson (employee_id = 1):</strong>

    <ul>
        <li>Tuần ngày 5-11 tháng Sáu (2023-06-05 đến 2023-06-11): 8.0 + 6.0 + 7.0 = 21.0 giờ (&gt; 20 giờ)</li>
        <li>Tuần ngày 12-18 tháng Sáu (2023-06-12 đến 2023-06-18): 12.0 + 9.0 = 21.0 giờ (&gt; 20 giờ)</li>
        <li>Có quá nhiều cuộc họp trong 2 tuần</li>
    </ul>
    </li>
    <li><strong>David Wilson (employee_id = 4):</strong>
    <ul>
        <li>Tuần ngày 5-11 tháng Sáu: 25.0 giờ (&gt; 20 giờ)</li>
        <li>Tuần ngày 19-25 tháng Sáu: 22.0 giờ (&gt; 20 giờ)</li>
        <li>Có quá nhiều cuộc họp trong 2 tuần</li>
    </ul>
    </li>
    <li><strong>Các nhân viên không được đưa vào kết quả:</strong>
    <ul>
        <li>Bob Smith (employee_id = 2): Tuần ngày 5-11 tháng Sáu: 15.0 + 8.0 = 23.0 giờ (&gt; 20), tuần ngày 12-18 tháng Sáu: 10.0 giờ (&lt; 20). Chỉ có 1 tuần có quá nhiều cuộc họp</li>
        <li>Carol Davis (employee_id = 3): Tuần ngày 5-11 tháng Sáu: 4.0 + 3.0 = 7.0 giờ (&lt; 20). Không có tuần nào có quá nhiều cuộc họp</li>
        <li>Emma Brown (employee_id = 5): Tuần ngày 5-11 tháng Sáu: 2.0 giờ (&lt; 20). Không có tuần nào có quá nhiều cuộc họp</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo meeting_heavy_weeks theo thứ tự giảm dần, sau đó theo tên nhân viên theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng hợp theo nhóm + truy vấn Join

<!-- thinking:start -->

> **Tư duy**
>
> Một tuần có quá nhiều cuộc họp được tính trên tổng theo tuần ISO của năm, chứ không phải trên một ngày riêng lẻ. Chuyển đổi ngày thành năm và tuần, sau đó tính tổng $\textit{duration}$ theo $(\textit{employee\_id},\textit{year},\textit{week})$.
>
> Giữ lại những tuần có ít nhất $20$ giờ, đếm số tuần đó cho từng nhân viên, nối những nhân viên có ít nhất hai tuần như vậy với bảng nhân viên, rồi sắp xếp theo số lượng giảm dần và tên tăng dần.
>
> Khóa tổng hợp phải bao gồm cả năm, nếu không cùng một số tuần ở các năm khác nhau sẽ bị gộp lại.

<!-- thinking:end -->

Đầu tiên, chúng ta nhóm dữ liệu theo `employee_id`, `year` và `week` để tính tổng số giờ họp của mỗi nhân viên trong từng tuần. Sau đó, chúng ta loại bỏ những tuần có số giờ họp vượt quá 20 và đếm số tuần có quá nhiều cuộc họp của từng nhân viên. Cuối cùng, chúng ta nối kết quả với bảng employees, lọc những nhân viên có ít nhất 2 tuần có quá nhiều cuộc họp, rồi sắp xếp kết quả theo yêu cầu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    week_meeting_hours AS (
        SELECT
            employee_id,
            YEAR(meeting_date) AS year,
            WEEK(meeting_date, 1) AS week,
            SUM(duration_hours) hours
        FROM meetings
        GROUP BY 1, 2, 3
    ),
    intensive_weeks AS (
        SELECT
            employee_id,
            employee_name,
            department,
            count(1) AS meeting_heavy_weeks
        FROM
            week_meeting_hours
            JOIN employees USING (employee_id)
        WHERE hours >= 20
        GROUP BY 1
    )
SELECT employee_id, employee_name, department, meeting_heavy_weeks
FROM intensive_weeks
WHERE meeting_heavy_weeks >= 2
ORDER BY 4 DESC, 2;
```

#### Pandas

```python
import pandas as pd


def find_overbooked_employees(
    employees: pd.DataFrame, meetings: pd.DataFrame
) -> pd.DataFrame:
    meetings["meeting_date"] = pd.to_datetime(meetings["meeting_date"])
    meetings["year"] = meetings["meeting_date"].dt.isocalendar().year
    meetings["week"] = meetings["meeting_date"].dt.isocalendar().week

    week_meeting_hours = (
        meetings.groupby(["employee_id", "year", "week"], as_index=False)[
            "duration_hours"
        ]
        .sum()
        .rename(columns={"duration_hours": "hours"})
    )

    intensive_weeks = week_meeting_hours[week_meeting_hours["hours"] >= 20]

    intensive_count = (
        intensive_weeks.groupby("employee_id")
        .size()
        .reset_index(name="meeting_heavy_weeks")
    )

    result = intensive_count.merge(employees, on="employee_id")

    result = result[result["meeting_heavy_weeks"] >= 2]

    result = result.sort_values(
        ["meeting_heavy_weeks", "employee_name"], ascending=[False, True]
    )

    return result[
        ["employee_id", "employee_name", "department", "meeting_heavy_weeks"]
    ].reset_index(drop=True)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
