---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [615. Average Salary Departments VS Company 🔒](https://leetcode.com/problems/average-salary-departments-vs-company)

[中文文档](/solution/0600-0699/0615.Average%20Salary%20Departments%20VS%20Company/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Salary</code></p>

<pre>
+-------------+------+
| Tên cột     | Kiểu |
+-------------+------+
| id          | int  |
| employee_id | int  |
| amount      | int  |
| pay_date    | date |
+-------------+------+
Trong SQL, id là cột khóa chính của bảng này.
Mỗi hàng trong bảng cho biết mức lương của một nhân viên trong một tháng.
employee_id là khóa ngoại (cột tham chiếu) đến bảng Employee.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Employee</code></p>

<pre>
+---------------+------+
| Tên cột       | Kiểu |
+---------------+------+
| employee_id   | int  |
| department_id | int  |
+---------------+------+
Trong SQL, employee_id là cột khóa chính của bảng này.
Mỗi hàng trong bảng cho biết phòng ban của một nhân viên.
</pre>

<p>&nbsp;</p>

<p>So sánh mức lương trung bình của nhân viên trong từng phòng ban với mức lương trung bình của toàn công ty, rồi trả về kết quả <strong>(higher/lower/same)</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Salary:
+----+-------------+--------+------------+
| id | employee_id | amount | pay_date   |
+----+-------------+--------+------------+
| 1  | 1           | 9000   | 2017/03/31 |
| 2  | 2           | 6000   | 2017/03/31 |
| 3  | 3           | 10000  | 2017/03/31 |
| 4  | 1           | 7000   | 2017/02/28 |
| 5  | 2           | 6000   | 2017/02/28 |
| 6  | 3           | 8000   | 2017/02/28 |
+----+-------------+--------+------------+
Bảng Employee:
+-------------+---------------+
| employee_id | department_id |
+-------------+---------------+
| 1           | 1             |
| 2           | 2             |
| 3           | 2             |
+-------------+---------------+
<strong>Đầu ra:</strong> 
+-----------+---------------+------------+
| pay_month | department_id | comparison |
+-----------+---------------+------------+
| 2017-02   | 1             | same       |
| 2017-03   | 1             | higher     |
| 2017-02   | 2             | same       |
| 2017-03   | 2             | lower      |
+-----------+---------------+------------+
<strong>Giải thích:</strong> 
Trong tháng 3, mức lương trung bình của công ty là (9000+6000+10000)/3 = 8333.33...
Mức lương trung bình của phòng ban &#39;1&#39; là 9000, cũng là mức lương của employee_id &#39;1&#39; vì phòng ban này chỉ có một nhân viên. Do 9000 &gt; 8333.33, kết quả so sánh là &#39;higher&#39;.
Mức lương trung bình của phòng ban &#39;2&#39; là (6000 + 10000)/2 = 8000, tức mức trung bình của employee_id &#39;2&#39; và &#39;3&#39;. Do 8000 &lt; 8333.33, kết quả so sánh là &#39;lower&#39;.

Áp dụng cùng cách so sánh cho tháng 2, kết quả là &#39;same&#39; vì mức lương trung bình của cả phòng ban &#39;1&#39; và &#39;2&#39; đều bằng mức trung bình của công ty là 7000.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi tháng cần so sánh mức trung bình của từng phòng ban với mức trung bình của công ty. Có thể dùng hai phép tổng hợp riêng, nhưng cách đó dài dòng.
>
> Tính trung bình bằng window function, phân vùng theo ngày trả lương (và theo ngày cùng phòng ban), để lấy cả hai mức trung bình trong một lượt. `CASE` so sánh hai giá trị, còn `DISTINCT` loại các hàng bị lặp do window function.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            DATE_FORMAT(pay_date, '%Y-%m') AS pay_month,
            department_id,
            AVG(amount) OVER (PARTITION BY pay_date) AS company_avg_amount,
            AVG(amount) OVER (PARTITION BY pay_date, department_id) AS department_avg_amount
        FROM
            Salary AS s
            JOIN Employee AS e ON s.employee_id = e.employee_id
    )
SELECT DISTINCT
    pay_month,
    department_id,
    CASE
        WHEN company_avg_amount = department_avg_amount THEN 'same'
        WHEN company_avg_amount < department_avg_amount THEN 'higher'
        ELSE 'lower'
    END AS comparison
FROM t;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng `DISTINCT` để gộp các hàng lặp do window function. Group by tháng và phòng ban, rồi lấy trung bình các giá trị đã tính bằng window function, sẽ cho cùng kết quả so sánh.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT *
        FROM
            Salary
            JOIN Employee USING (employee_id)
    ),
    T AS (
        SELECT
            DATE_FORMAT(pay_date, '%Y-%m') AS pay_month,
            department_id,
            AVG(amount) OVER (PARTITION BY pay_date, department_id) AS department_avg,
            AVG(amount) OVER (PARTITION BY pay_date) AS company_avg
        FROM S
    )
SELECT
    pay_month,
    department_id,
    CASE
        WHEN AVG(department_avg) > AVG(company_avg) THEN 'higher'
        WHEN AVG(department_avg) < AVG(company_avg) THEN 'lower'
        ELSE 'same'
    END AS comparison
FROM T
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
