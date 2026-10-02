---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [569. Median Employee Salary 🔒](https://leetcode.com/problems/median-employee-salary)

[中文文档](/solution/0500-0599/0569.Median%20Employee%20Salary/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Employee</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| company      | varchar |
| salary       | int     |
+--------------+---------+
id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết công ty và mức lương của một nhân viên.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm các hàng chứa mức lương trung vị của từng công ty. Khi sắp xếp mức lương của công ty để tính trung vị, nếu bằng nhau thì dùng <code>id</code> để phân định thứ tự.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Employee:
+----+---------+--------+
| id | company | salary |
+----+---------+--------+
| 1  | A       | 2341   |
| 2  | A       | 341    |
| 3  | A       | 15     |
| 4  | A       | 15314  |
| 5  | A       | 451    |
| 6  | A       | 513    |
| 7  | B       | 15     |
| 8  | B       | 13     |
| 9  | B       | 1154   |
| 10 | B       | 1345   |
| 11 | B       | 1221   |
| 12 | B       | 234    |
| 13 | C       | 2345   |
| 14 | C       | 2645   |
| 15 | C       | 2645   |
| 16 | C       | 2652   |
| 17 | C       | 65     |
+----+---------+--------+
<strong>Đầu ra:</strong> 
+----+---------+--------+
| id | company | salary |
+----+---------+--------+
| 5  | A       | 451    |
| 6  | A       | 513    |
| 12 | B       | 234    |
| 9  | B       | 1154   |
| 14 | C       | 2645   |
+----+---------+--------+
<strong>Giải thích:</strong> 
Các hàng của công ty A sau khi sắp xếp:
+----+---------+--------+
| id | company | salary |
+----+---------+--------+
| 3  | A       | 15     |
| 2  | A       | 341    |
| 5  | A       | 451    | &lt;-- trung vị
| 6  | A       | 513    | &lt;-- trung vị
| 1  | A       | 2341   |
| 4  | A       | 15314  |
+----+---------+--------+
Các hàng của công ty B sau khi sắp xếp:
+----+---------+--------+
| id | company | salary |
+----+---------+--------+
| 8  | B       | 13     |
| 7  | B       | 15     |
| 12 | B       | 234    | &lt;-- trung vị
| 11 | B       | 1221   | &lt;-- trung vị
| 9  | B       | 1154   |
| 10 | B       | 1345   |
+----+---------+--------+
Các hàng của công ty C sau khi sắp xếp:
+----+---------+--------+
| id | company | salary |
+----+---------+--------+
| 17 | C       | 65     |
| 13 | C       | 2345   |
| 14 | C       | 2645   | &lt;-- trung vị
| 15 | C       | 2645   | 
| 16 | C       | 2652   |
+----+---------+--------+
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này mà không dùng hàm built-in hoặc window function không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trung vị của mỗi công ty là mức lương ở giữa, hoặc hai mức lương ở giữa nếu số lượng là chẵn. Số thứ tự hàng sau khi sắp xếp tương ứng với các vị trí này.
>
> `ROW_NUMBER()` đánh số thứ tự mức lương trong từng công ty, còn `COUNT` cho biết số lượng $n$. Giữ các hàng có thứ hạng nằm trong $[n/2,\ n/2+1]$: một hàng nếu $n$ lẻ, hai hàng nếu $n$ chẵn. Không cần self-join.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            *,
            ROW_NUMBER() OVER (
                PARTITION BY company
                ORDER BY salary ASC
            ) AS rk,
            COUNT(id) OVER (PARTITION BY company) AS n
        FROM Employee
    )
SELECT
    id,
    company,
    salary
FROM t
WHERE rk >= n / 2 AND rk <= n / 2 + 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
