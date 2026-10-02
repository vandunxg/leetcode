---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [579. Find Cumulative Salary of an Employee 🔒](https://leetcode.com/problems/find-cumulative-salary-of-an-employee)

[中文文档](/solution/0500-0599/0579.Find%20Cumulative%20Salary%20of%20an%20Employee/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employee</code></p>

<pre>
+-------------+------+
| Tên cột     | Kiểu |
+-------------+------+
| id          | int  |
| month       | int  |
| salary      | int  |
+-------------+------+
(id, month) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết mức lương của một nhân viên trong một tháng của năm 2020.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tính <strong>bảng tổng hợp lương lũy kế</strong> cho mỗi nhân viên trong một bảng thống nhất.</p>

<p>Có thể tính <strong>bảng tổng hợp lương lũy kế</strong> cho một nhân viên như sau:</p>

<ul>
	<li>Với mỗi tháng nhân viên làm việc, hãy <strong>cộng</strong> lương của <strong>tháng đó</strong> và <strong>hai tháng trước</strong>. Đây là <strong>tổng lương 3 tháng</strong> tính cho tháng đó. Nếu nhân viên không làm việc cho công ty trong các tháng trước, mức lương tương ứng của những tháng đó là <code>0</code>.</li>
	<li><strong>Không</strong> đưa tổng lương 3 tháng của <strong>tháng gần nhất</strong> mà nhân viên làm việc vào bảng tổng hợp.</li>
	<li><strong>Không</strong> đưa tổng lương 3 tháng của bất kỳ tháng nào mà nhân viên <strong>không làm việc</strong> vào bảng tổng hợp.</li>
</ul>

<p>Trả về bảng kết quả được sắp xếp theo <code>id</code> theo <strong>thứ tự tăng dần</strong>. Nếu trùng nhau, sắp xếp theo <code>month</code> theo <strong>thứ tự giảm dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Employee:
+----+-------+--------+
| id | month | salary |
+----+-------+--------+
| 1  | 1     | 20     |
| 2  | 1     | 20     |
| 1  | 2     | 30     |
| 2  | 2     | 30     |
| 3  | 2     | 40     |
| 1  | 3     | 40     |
| 3  | 3     | 60     |
| 1  | 4     | 60     |
| 3  | 4     | 70     |
| 1  | 7     | 90     |
| 1  | 8     | 90     |
+----+-------+--------+
<strong>Đầu ra:</strong> 
+----+-------+--------+
| id | month | Salary |
+----+-------+--------+
| 1  | 7     | 90     |
| 1  | 4     | 130    |
| 1  | 3     | 90     |
| 1  | 2     | 50     |
| 1  | 1     | 20     |
| 2  | 1     | 20     |
| 3  | 3     | 100    |
| 3  | 2     | 40     |
+----+-------+--------+
<strong>Giải thích:</strong> 
Nhân viên &#39;1&#39; có năm bản ghi lương nếu không tính tháng gần nhất là &#39;8&#39;:
- 90 cho tháng &#39;7&#39;.
- 60 cho tháng &#39;4&#39;.
- 40 cho tháng &#39;3&#39;.
- 30 cho tháng &#39;2&#39;.
- 20 cho tháng &#39;1&#39;.
Vì vậy, bảng tổng hợp lương lũy kế của nhân viên này là:
+----+-------+--------+
| id | month | salary |
+----+-------+--------+
| 1  | 7     | 90     |  (90 + 0 + 0)
| 1  | 4     | 130    |  (60 + 40 + 30)
| 1  | 3     | 90     |  (40 + 30 + 20)
| 1  | 2     | 50     |  (30 + 20 + 0)
| 1  | 1     | 20     |  (20 + 0 + 0)
+----+-------+--------+
Lưu ý, tổng lương 3 tháng của tháng &#39;7&#39; là 90 vì nhân viên không làm việc trong tháng &#39;6&#39; và tháng &#39;5&#39;.

Nhân viên &#39;2&#39; chỉ còn một bản ghi lương (tháng &#39;1&#39;) sau khi loại tháng gần nhất là tháng &#39;2&#39;.
+----+-------+--------+
| id | month | salary |
+----+-------+--------+
| 2  | 1     | 20     |  (20 + 0 + 0)
+----+-------+--------+

Nhân viên &#39;3&#39; có hai bản ghi lương nếu không tính tháng gần nhất là &#39;4&#39;:
- 60 cho tháng &#39;3&#39;.
- 40 cho tháng &#39;2&#39;.
Vì vậy, bảng tổng hợp lương lũy kế của nhân viên này là:
+----+-------+--------+
| id | month | salary |
+----+-------+--------+
| 3  | 3     | 100    |  (60 + 40 + 0)
| 3  | 2     | 40     |  (40 + 0 + 0)
+----+-------+--------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi loại tháng gần nhất của mỗi nhân viên, cộng lương tháng đó với lương hai tháng trước. `RANGE 2 PRECEDING` tính dựa trên giá trị tháng, không phải số lượng hàng.
>
> Loại `(id, MAX(month))`, sau đó tính tổng cửa sổ theo từng id, sắp xếp theo month. Sắp xếp kết quả theo id tăng dần và month giảm dần.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    id,
    month,
    SUM(salary) OVER (
        PARTITION BY id
        ORDER BY month
        RANGE 2 PRECEDING
    ) AS Salary
FROM employee
WHERE
    (id, month) NOT IN (
        SELECT
            id,
            MAX(month)
        FROM Employee
        GROUP BY id
    )
ORDER BY id, month DESC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 loại tháng gần nhất bằng `NOT IN`. Ta cũng có thể xếp hạng các tháng theo thứ tự giảm dần rồi giữ lại các hàng có $rk>1$.
>
> `RANK() OVER (... ORDER BY month DESC)` đánh dấu tháng gần nhất bằng $1$. Cửa sổ tính lũy kế giống Lời giải 1; cách lọc này trực tiếp hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            id,
            month,
            SUM(salary) OVER (
                PARTITION BY id
                ORDER BY month
                RANGE 2 PRECEDING
            ) AS salary,
            RANK() OVER (
                PARTITION BY id
                ORDER BY month DESC
            ) AS rk
        FROM Employee
    )
SELECT id, month, salary
FROM T
WHERE rk > 1
ORDER BY 1, 2 DESC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
