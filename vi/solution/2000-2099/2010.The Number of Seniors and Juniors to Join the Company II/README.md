---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2010. The Number of Seniors and Juniors to Join the Company II 🔒](https://leetcode.com/problems/the-number-of-seniors-and-juniors-to-join-the-company-ii)

[中文文档](/solution/2000-2099/2010.The%20Number%20of%20Seniors%20and%20Juniors%20to%20Join%20the%20Company%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Candidates</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| employee_id | int  |
| experience  | enum |
| salary      | int  |
+-------------+------+
employee_id là cột có các giá trị duy nhất trong bảng này.
experience là một ENUM (nhóm) gồm các loại (&#39;Senior&#39;, &#39;Junior&#39;).
Mỗi hàng trong bảng này cho biết id của một ứng viên, mức lương hàng tháng và kinh nghiệm của họ.
Mức lương của mỗi ứng viên được đảm bảo là <strong>duy nhất</strong>.</pre>

<p>&nbsp;</p>

<p>Một công ty muốn tuyển nhân viên mới. Ngân sách dành cho lương của công ty là <code>$70000</code>. Tiêu chí tuyển dụng của công ty là:</p>

<ol>
	<li>Tiếp tục tuyển senior có mức lương thấp nhất cho đến khi không thể tuyển thêm senior.</li>
	<li>Dùng ngân sách còn lại để tuyển junior có mức lương thấp nhất.</li>
	<li>Tiếp tục tuyển junior có mức lương thấp nhất cho đến khi không thể tuyển thêm junior.</li>
</ol>

<p>Hãy viết lời giải để tìm id của các senior và junior được tuyển theo các tiêu chí trên.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Candidates:
+-------------+------------+--------+
| employee_id | experience | salary |
+-------------+------------+--------+
| 1           | Junior     | 10000  |
| 9           | Junior     | 15000  |
| 2           | Senior     | 20000  |
| 11          | Senior     | 16000  |
| 13          | Senior     | 50000  |
| 4           | Junior     | 40000  |
+-------------+------------+--------+
<strong>Đầu ra:</strong>
+-------------+
| employee_id |
+-------------+
| 11          |
| 2           |
| 1           |
| 9           |
+-------------+
<strong>Giải thích:</strong>
Chúng ta có thể tuyển 2 senior với các ID (11, 2). Vì ngân sách là $70000 and the sum of their salaries is $36000, chúng ta còn $34000 nhưng không đủ để tuyển ứng viên senior có ID 13.
Chúng ta có thể tuyển 2 junior với các ID (1, 9). Vì ngân sách còn lại là $34000 and the sum of their salaries is $25000, chúng ta còn $9000 nhưng không đủ để tuyển ứng viên junior có ID 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Candidates:
+-------------+------------+--------+
| employee_id | experience | salary |
+-------------+------------+--------+
| 1           | Junior     | 25000  |
| 9           | Junior     | 10000  |
| 2           | Senior     | 85000  |
| 11          | Senior     | 80000  |
| 13          | Senior     | 90000  |
| 4           | Junior     | 30000  |
+-------------+------------+--------+
<strong>Đầu ra:</strong>
+-------------+
| employee_id |
+-------------+
| 9           |
| 1           |
| 4           |
+-------------+
<strong>Giải thích:</strong>
Với ngân sách hiện tại, chúng ta không thể tuyển senior nào vì cần ít nhất $80000 để tuyển một senior.
Chúng ta có thể tuyển cả ba junior với ngân sách còn lại.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc tuyển dụng giống bài 2004, nhưng thay vì số lượng, chúng ta trả về các $employee\_id$s được chấp nhận. Tổng prefix bằng window vẫn kiểm tra được liệu việc tuyển đến một người có còn nằm trong ngân sách hay không.
>
> Senior được chi trả trước; tổng chạy của junior sẽ cộng thêm khoản chi đó. Chọn các id với $cur \le 70000$ và `UNION` chúng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    s AS (
        SELECT
            employee_id,
            SUM(salary) OVER (ORDER BY salary) AS cur
        FROM Candidates
        WHERE experience = 'Senior'
    ),
    j AS (
        SELECT
            employee_id,
            IFNULL(
                SELECT
                    MAX(cur)
                FROM s
                WHERE cur <= 70000,
                0
            ) + SUM(salary) OVER (ORDER BY salary) AS cur
        FROM Candidates
        WHERE experience = 'Junior'
    )
SELECT
    employee_id
FROM s
WHERE cur <= 70000
UNION
SELECT
    employee_id
FROM j
WHERE cur <= 70000;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
