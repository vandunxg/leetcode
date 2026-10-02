---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1468. Calculate Salaries 🔒](https://leetcode.com/problems/calculate-salaries)

[中文文档](/solution/1400-1499/1468.Calculate%20Salaries/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng <code>Salaries</code>:</p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| company_id    | int     |
| employee_id   | int     |
| employee_name | varchar |
| salary        | int     |
+---------------+---------+
Trong SQL, (company_id, employee_id) là khóa chính của bảng này.
Bảng này chứa mã công ty, mã nhân viên, tên nhân viên và lương của nhân viên.
</pre>

<p>&nbsp;</p>

<p>Hãy tìm mức lương của các nhân viên sau khi áp dụng thuế. Làm tròn mức lương đến <strong>số nguyên gần nhất</strong>.</p>

<p>Mức thuế được tính cho từng công ty dựa trên các tiêu chí sau:</p>

<ul>
	<li><code>0%</code> nếu mức lương cao nhất của bất kỳ nhân viên nào trong công ty nhỏ hơn <code>$1000</code>.</li>
	<li><code>24%</code> nếu mức lương cao nhất của bất kỳ nhân viên nào trong công ty nằm trong khoảng <code>[1000, 10000]</code>, bao gồm cả hai đầu mút.</li>
	<li><code>49%</code> nếu mức lương cao nhất của bất kỳ nhân viên nào trong công ty lớn hơn <code>$10000</code>.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Salaries:
+------------+-------------+---------------+--------+
| company_id | employee_id | employee_name | salary |
+------------+-------------+---------------+--------+
| 1          | 1           | Tony          | 2000   |
| 1          | 2           | Pronub        | 21300  |
| 1          | 3           | Tyrrox        | 10800  |
| 2          | 1           | Pam           | 300    |
| 2          | 7           | Bassem        | 450    |
| 2          | 9           | Hermione      | 700    |
| 3          | 7           | Bocaben       | 100    |
| 3          | 2           | Ognjen        | 2200   |
| 3          | 13          | Nyancat       | 3300   |
| 3          | 15          | Morninngcat   | 7777   |
+------------+-------------+---------------+--------+
<strong>Đầu ra:</strong>
+------------+-------------+---------------+--------+
| company_id | employee_id | employee_name | salary |
+------------+-------------+---------------+--------+
| 1          | 1           | Tony          | 1020   |
| 1          | 2           | Pronub        | 10863  |
| 1          | 3           | Tyrrox        | 5508   |
| 2          | 1           | Pam           | 300    |
| 2          | 7           | Bassem        | 450    |
| 2          | 9           | Hermione      | 700    |
| 3          | 7           | Bocaben       | 76     |
| 3          | 2           | Ognjen        | 1672   |
| 3          | 13          | Nyancat       | 2508   |
| 3          | 15          | Morninngcat   | 5911   |
+------------+-------------+---------------+--------+
<strong>Giải thích:</strong>
Với công ty 1, mức lương cao nhất là 21300. Nhân viên trong công ty 1 chịu thuế = 49%
Với công ty 2, mức lương cao nhất là 700. Nhân viên trong công ty 2 chịu thuế = 0%
Với công ty 3, mức lương cao nhất là 7777. Nhân viên trong công ty 3 chịu thuế = 24%
Mức lương sau thuế = salary - (tỷ lệ thuế / 100) * salary
Ví dụ, mức lương sau thuế của Morninngcat (3, 15) = 7777 - 7777 * (24 / 100) = 7777 - 1866.48 = 5910.52, sau khi làm tròn là 5911.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mức thuế phụ thuộc vào mức lương cao nhất của công ty. Gom nhóm để tính `MAX(salary)` cho từng công ty, nối kết quả trở lại bảng ban đầu, rồi áp dụng các khoảng $1000$/$10000$ bằng `CASE` và `ROUND`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    s.company_id,
    employee_id,
    employee_name,
    ROUND(
        CASE
            WHEN top < 1000 THEN salary
            WHEN top >= 1000
            AND top <= 10000 THEN salary * 0.76
            ELSE salary * 0.51
        END
    ) AS salary
FROM
    Salaries AS s
    JOIN (
        SELECT company_id, MAX(salary) AS top
        FROM Salaries
        GROUP BY company_id
    ) AS t
        ON s.company_id = t.company_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
