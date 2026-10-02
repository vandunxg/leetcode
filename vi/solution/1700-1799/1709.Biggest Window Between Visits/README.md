---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1709. Biggest Window Between Visits 🔒](https://leetcode.com/problems/biggest-window-between-visits)

[中文文档](/solution/1700-1799/1709.Biggest%20Window%20Between%20Visits/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>UserVisits</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| visit_date  | date |
+-------------+------+
Bảng này không có khóa chính và có thể chứa các hàng trùng lặp.
Bảng này chứa nhật ký ngày người dùng ghé thăm một nhà bán lẻ.
</pre>

<p>&nbsp;</p>

<p>Giả sử ngày hiện tại là <code>&#39;2021-1-1&#39;</code>.</p>

<p>Viết lời giải để với mỗi <code>user_id</code>, tìm <code>window</code> lớn nhất tính theo số ngày giữa mỗi lần ghé thăm và lần ghé thăm ngay sau đó (hoặc ngày hiện tại nếu đó là lần cuối).</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>user_id</code>.</p>

<p>Định dạng kết quả truy vấn như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
UserVisits table:
+---------+------------+
| user_id | visit_date |
+---------+------------+
| 1       | 2020-11-28 |
| 1       | 2020-10-20 |
| 1       | 2020-12-3  |
| 2       | 2020-10-5  |
| 2       | 2020-12-9  |
| 3       | 2020-11-11 |
+---------+------------+
<strong>Đầu ra:</strong>
+---------+---------------+
| user_id | biggest_window|
+---------+---------------+
| 1       | 39            |
| 2       | 65            |
| 3       | 51            |
+---------+---------------+
<strong>Giải thích:</strong>
Với người dùng thứ nhất, các khoảng thời gian là:
    - Từ 2020-10-20 đến 2020-11-28, tổng cộng 39 ngày.
    - Từ 2020-11-28 đến 2020-12-3, tổng cộng 5 ngày.
    - Từ 2020-12-3 đến 2021-1-1, tổng cộng 29 ngày.
Khoảng lớn nhất là 39 ngày.
Với người dùng thứ hai, các khoảng thời gian là:
    - Từ 2020-10-5 đến 2020-12-9, tổng cộng 65 ngày.
    - Từ 2020-12-9 đến 2021-1-1, tổng cộng 23 ngày.
Khoảng lớn nhất là 65 ngày.
Với người dùng thứ ba, khoảng duy nhất là từ 2020-11-11 đến 2021-1-1, tổng cộng 51 ngày.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách ghé thăm lớn nhất của một người dùng là hiệu lớn nhất giữa hai ngày ghé thăm liên tiếp, trong đó lần ghé thăm cuối được so với $2021$-$01$-$01$.
>
> $\mathrm{LEAD}$ phân nhóm theo $\textit{user\_id}$ và sắp xếp theo ngày sẽ cho lần ghé thăm tiếp theo (mặc định là $2021$-$1$-$1$). $\mathrm{DATEDIFF}$ tính các khoảng cách, còn $\mathrm{MAX}$ theo từng người dùng là đáp án.

<!-- thinking:end -->

Ta có thể dùng hàm cửa sổ `LEAD` để lấy ngày ghé thăm tiếp theo của mỗi người dùng (nếu không có lần tiếp theo thì xem là `2021-1-1`), rồi dùng hàm `DATEDIFF` để tính số ngày giữa hai lần ghé thăm. Cuối cùng, lấy giá trị lớn nhất của số ngày giữa các lần ghé thăm theo từng người dùng.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            user_id,
            DATEDIFF(
                LEAD(visit_date, 1, '2021-1-1') OVER (
                    PARTITION BY user_id
                    ORDER BY visit_date
                ),
                visit_date
            ) AS diff
        FROM UserVisits
    )
SELECT user_id, MAX(diff) AS biggest_window
FROM T
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
