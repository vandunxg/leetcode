---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1179. Reformat Department Table](https://leetcode.com/problems/reformat-department-table)

[中文文档](/solution/1100-1199/1179.Reformat%20Department%20Table/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Department</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| revenue     | int     |
| month       | varchar |
+-------------+---------+
Trong SQL, (id, month) là primary key của bảng này.
Bảng chứa doanh thu hàng tháng của từng phòng ban.
month nhận các giá trị [&quot;Jan&quot;,&quot;Feb&quot;,&quot;Mar&quot;,&quot;Apr&quot;,&quot;May&quot;,&quot;Jun&quot;,&quot;Jul&quot;,&quot;Aug&quot;,&quot;Sep&quot;,&quot;Oct&quot;,&quot;Nov&quot;,&quot;Dec&quot;].
</pre>

<p>&nbsp;</p>

<p>Hãy định dạng lại bảng sao cho có một cột id phòng ban và một cột doanh thu <strong>cho mỗi tháng</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Department:
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+
<strong>Đầu ra:</strong> 
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
<strong>Giải thích:</strong> Doanh thu từ Apr đến Dec là null.
Lưu ý bảng kết quả có 13 cột (1 cột id phòng ban và 12 cột tương ứng với các tháng).
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Dữ liệu doanh thu mỗi tháng trên một hàng cần được chuyển thành một hàng cho mỗi phòng ban với mười hai cột. `GROUP BY id` cùng `SUM(CASE month WHEN ...)` chọn doanh thu của tháng tương ứng; vì mỗi tháng có nhiều nhất một hàng nên `SUM` trả về giá trị đó, hoặc `NULL` nếu không có dữ liệu.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    id,
    SUM(
        CASE month
            WHEN 'Jan' THEN revenue
        END
    ) AS Jan_Revenue,
    SUM(
        CASE month
            WHEN 'Feb' THEN revenue
        END
    ) AS Feb_Revenue,
    SUM(
        CASE month
            WHEN 'Mar' THEN revenue
        END
    ) AS Mar_Revenue,
    SUM(
        CASE month
            WHEN 'Apr' THEN revenue
        END
    ) AS Apr_Revenue,
    SUM(
        CASE month
            WHEN 'May' THEN revenue
        END
    ) AS May_Revenue,
    SUM(
        CASE month
            WHEN 'Jun' THEN revenue
        END
    ) AS Jun_Revenue,
    SUM(
        CASE month
            WHEN 'Jul' THEN revenue
        END
    ) AS Jul_Revenue,
    SUM(
        CASE month
            WHEN 'Aug' THEN revenue
        END
    ) AS Aug_Revenue,
    SUM(
        CASE month
            WHEN 'Sep' THEN revenue
        END
    ) AS Sep_Revenue,
    SUM(
        CASE month
            WHEN 'Oct' THEN revenue
        END
    ) AS Oct_Revenue,
    SUM(
        CASE month
            WHEN 'Nov' THEN revenue
        END
    ) AS Nov_Revenue,
    SUM(
        CASE month
            WHEN 'Dec' THEN revenue
        END
    ) AS Dec_Revenue
FROM Department
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
