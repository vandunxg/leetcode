---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2494. Merge Overlapping Events in the Same Hall 🔒](https://leetcode.com/problems/merge-overlapping-events-in-the-same-hall)

[中文文档](/solution/2400-2499/2494.Merge%20Overlapping%20Events%20in%20the%20Same%20Hall/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>HallEvents</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| hall_id     | int  |
| start_day   | date |
| end_day     | date |
+-------------+------+
Bảng này có thể chứa các hàng trùng lặp.
Mỗi hàng của bảng này cho biết ngày bắt đầu, ngày kết thúc của một event và hall nơi event được tổ chức.
</pre>

<p>&nbsp;</p>

<p>Viết một lời giải để gộp tất cả các event chồng lấn được tổ chức <strong>trong cùng một hall</strong>. Hai event chồng lấn nếu chúng có <strong>ít nhất một ngày</strong> chung.</p>

<p>Trả về bảng kết quả <strong>theo bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng HallEvents:
+---------+------------+------------+
| hall_id | start_day  | end_day    |
+---------+------------+------------+
| 1       | 2023-01-13 | 2023-01-14 |
| 1       | 2023-01-14 | 2023-01-17 |
| 1       | 2023-01-18 | 2023-01-25 |
| 2       | 2022-12-09 | 2022-12-23 |
| 2       | 2022-12-13 | 2022-12-17 |
| 3       | 2022-12-01 | 2023-01-30 |
+---------+------------+------------+
<strong>Đầu ra:</strong>
+---------+------------+------------+
| hall_id | start_day  | end_day    |
+---------+------------+------------+
| 1       | 2023-01-13 | 2023-01-17 |
| 1       | 2023-01-18 | 2023-01-25 |
| 2       | 2022-12-09 | 2022-12-23 |
| 3       | 2022-12-01 | 2023-01-30 |
+---------+------------+------------+
<strong>Giải thích:</strong> Có ba hall.
Hall 1:
- Hai event [&quot;2023-01-13&quot;, &quot;2023-01-14&quot;] và [&quot;2023-01-14&quot;, &quot;2023-01-17&quot;] chồng lấn. Ta gộp chúng thành một event [&quot;2023-01-13&quot;, &quot;2023-01-17&quot;].
- Event [&quot;2023-01-18&quot;, &quot;2023-01-25&quot;] không chồng lấn với event nào khác, nên giữ nguyên.
Hall 2:
- Hai event [&quot;2022-12-09&quot;, &quot;2022-12-23&quot;] và [&quot;2022-12-13&quot;, &quot;2022-12-17&quot;] chồng lấn. Ta gộp chúng thành một event [&quot;2022-12-09&quot;, &quot;2022-12-23&quot;].
Hall 3:
- Hall chỉ có một event, nên ta trả về event đó. Lưu ý rằng ta chỉ xét các event của từng hall một cách độc lập.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trong mỗi hall, gộp các event chồng lấn hoặc tiếp giáp sau khi sắp xếp theo start day. Một $\textit{MAX}(\textit{end_day})$ đang chạy là end mới nhất cho đến thời điểm hiện tại; nếu start tiếp theo vẫn $\le$ giá trị đó, nó thuộc cùng một nhóm.
>
> $\textit{LAG}$ đánh dấu nhóm mới, $\textit{SUM}$ gán mã nhóm, sau đó $\textit{MIN}$/$\textit{MAX}$ hoàn tất mỗi nhóm.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT
            hall_id,
            start_day,
            end_day,
            MAX(end_day) OVER (
                PARTITION BY hall_id
                ORDER BY start_day
            ) AS cur_max_end_day
        FROM HallEvents
    ),
    T AS (
        SELECT
            *,
            IF(
                start_day <= LAG(cur_max_end_day) OVER (
                    PARTITION BY hall_id
                    ORDER BY start_day
                ),
                0,
                1
            ) AS start
        FROM S
    ),
    P AS (
        SELECT
            *,
            SUM(start) OVER (
                PARTITION BY hall_id
                ORDER BY start_day
            ) AS gid
        FROM T
    )
SELECT hall_id, MIN(start_day) AS start_day, MAX(end_day) AS end_day
FROM P
GROUP BY hall_id, gid;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
