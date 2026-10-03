---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2004. The Number of Seniors and Juniors to Join the Company 🔒](https://leetcode.com/problems/the-number-of-seniors-and-juniors-to-join-the-company)

[中文文档](/solution/2000-2099/2004.The%20Number%20of%20Seniors%20and%20Juniors%20to%20Join%20the%20Company/README.md)

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
employee_id là cột chứa các giá trị duy nhất trong bảng này.
experience là kiểu ENUM (danh mục), với các giá trị (&#39;Senior&#39;, &#39;Junior&#39;).
Mỗi hàng của bảng này cho biết ID, mức lương hàng tháng và kinh nghiệm của một ứng viên.
</pre>

<p>&nbsp;</p>

<p>Một công ty muốn tuyển nhân viên mới. Ngân sách dành cho tiền lương của công ty là <code>$70000</code>. Tiêu chí tuyển dụng của công ty là:</p>

<ol>
	<li>Tuyển số lượng Senior lớn nhất.</li>
	<li>Sau khi tuyển số lượng Senior tối đa, dùng ngân sách còn lại để tuyển số lượng Junior lớn nhất.</li>
</ol>

<p>Hãy viết lời giải để tìm số lượng Senior và Junior được tuyển theo các tiêu chí trên.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Candidates:
+-------------+------------+--------+
| employee_id | experience | salary |
+-------------+------------+--------+
| 1           | Junior     | 10000  |
| 9           | Junior     | 10000  |
| 2           | Senior     | 20000  |
| 11          | Senior     | 20000  |
| 13          | Senior     | 50000  |
| 4           | Junior     | 40000  |
+-------------+------------+--------+
<strong>Đầu ra:</strong>
+------------+---------------------+
| experience | accepted_candidates |
+------------+---------------------+
| Senior     | 2                   |
| Junior     | 2                   |
+------------+---------------------+
<strong>Giải thích:</strong>
Ta có thể tuyển 2 Senior có ID (2, 11). Vì ngân sách là $70000 and the sum of their salaries is $40000, chúng ta vẫn còn $30000 nhưng không đủ để tuyển ứng viên Senior có ID 13.
Ta có thể tuyển 2 Junior có ID (1, 9). Vì ngân sách còn lại là $30000 and the sum of their salaries is $20000, chúng ta vẫn còn $10000 nhưng không đủ để tuyển ứng viên Junior có ID 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Candidates:
+-------------+------------+--------+
| employee_id | experience | salary |
+-------------+------------+--------+
| 1           | Junior     | 10000  |
| 9           | Junior     | 10000  |
| 2           | Senior     | 80000  |
| 11          | Senior     | 80000  |
| 13          | Senior     | 80000  |
| 4           | Junior     | 40000  |
+-------------+------------+--------+
<strong>Đầu ra:</strong>
+------------+---------------------+
| experience | accepted_candidates |
+------------+---------------------+
| Senior     | 0                   |
| Junior     | 3                   |
+------------+---------------------+
<strong>Giải thích:</strong>
Với ngân sách hiện tại, chúng ta không thể tuyển Senior nào vì cần ít nhất $80000 để tuyển một Senior.
Với ngân sách còn lại, chúng ta có thể tuyển cả ba Junior.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ngân sách cố định ở mức $70000$ và Senior được tuyển trước Junior. Việc liệt kê mọi tập con không đáp ứng được quy mô bài toán. Trong mỗi nhóm, tuyển theo thứ tự lương tăng dần sẽ tối đa hóa số lượng nhân viên.
>
> Với mỗi nhóm, $cur$ là tổng lũy kế chi phí tuyển đến nhân viên đó. Các Senior có $cur \le 70000$ được chấp nhận; với Junior, phần chi phí của Senior được cộng trước.
>
> Hai CTE tính các tổng lũy kế đó, sau đó `UNION ALL` đếm số hàng còn nằm trong ngân sách.

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
    'Senior' AS experience,
    COUNT(employee_id) AS accepted_candidates
FROM s
WHERE cur <= 70000
UNION ALL
SELECT
    'Junior' AS experience,
    COUNT(employee_id) AS accepted_candidates
FROM j
WHERE cur <= 70000;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
