---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1369. Get the Second Most Recent Activity 🔒](https://leetcode.com/problems/get-the-second-most-recent-activity)

[中文文档](/solution/1300-1399/1369.Get%20the%20Second%20Most%20Recent%20Activity/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>UserActivity</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| username      | varchar |
| activity      | varchar |
| startDate     | Date    |
| endDate       | Date    |
+---------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Bảng này chứa thông tin về hoạt động mà mỗi người dùng thực hiện trong một khoảng thời gian.
Người dùng có username đã thực hiện một hoạt động từ startDate đến endDate.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để hiển thị <strong>hoạt động gần đây thứ hai</strong> của mỗi người dùng.</p>

<p>Nếu người dùng chỉ có một hoạt động, hãy trả về hoạt động đó. Mỗi người dùng không thể thực hiện nhiều hoạt động cùng lúc.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng UserActivity:
+------------+--------------+-------------+-------------+
| username   | activity     | startDate   | endDate     |
+------------+--------------+-------------+-------------+
| Alice      | Travel       | 2020-02-12  | 2020-02-20  |
| Alice      | Dancing      | 2020-02-21  | 2020-02-23  |
| Alice      | Travel       | 2020-02-24  | 2020-02-28  |
| Bob        | Travel       | 2020-02-11  | 2020-02-18  |
+------------+--------------+-------------+-------------+
<strong>Đầu ra:</strong> 
+------------+--------------+-------------+-------------+
| username   | activity     | startDate   | endDate     |
+------------+--------------+-------------+-------------+
| Alice      | Dancing      | 2020-02-21  | 2020-02-23  |
| Bob        | Travel       | 2020-02-11  | 2020-02-18  |
+------------+--------------+-------------+-------------+
<strong>Giải thích:</strong> 
Hoạt động gần đây nhất của Alice là Travel, từ 2020-02-24 đến 2020-02-28; trước đó, cô ấy tham gia Dancing từ 2020-02-21 đến 2020-02-23.
Bob chỉ có một bản ghi, nên ta lấy bản ghi đó.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi người dùng, cần lấy hoạt động gần đây thứ hai, hoặc hoạt động duy nhất nếu họ chỉ có một hàng. Dùng window function $\mathrm{RANK}$ phân vùng theo người dùng và sắp xếp ngày bắt đầu giảm dần, kết hợp với window function $\mathrm{COUNT}$, ta có thể giữ lại hàng có rank $2$ hoặc tổng số hàng bằng $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    username,
    activity,
    startdate,
    enddate
FROM
    (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY username
                ORDER BY startdate DESC
            ) AS rk,
            COUNT(username) OVER (PARTITION BY username) AS cnt
        FROM UserActivity
    ) AS a
WHERE a.rk = 2 OR a.cnt = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
