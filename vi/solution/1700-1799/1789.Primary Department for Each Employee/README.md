---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1789. Primary Department for Each Employee](https://leetcode.com/problems/primary-department-for-each-employee)

[中文文档](/solution/1700-1799/1789.Primary%20Department%20for%20Each%20Employee/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Employee</code></p>

<pre>
+---------------+---------+
| Column Name   |  Type   |
+---------------+---------+
| employee_id   | int     |
| department_id | int     |
| primary_flag  | varchar |
+---------------+---------+
(employee_id, department_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
employee_id là ID của nhân viên.
department_id là ID của phòng ban mà nhân viên thuộc về.
primary_flag là một ENUM (kiểu phân loại) có dạng (&#39;Y&#39;, &#39;N&#39;). Nếu cờ là &#39;Y&#39;, phòng ban là phòng ban chính của nhân viên. Nếu cờ là &#39;N&#39;, đó không phải phòng ban chính.
</pre>

<p>&nbsp;</p>

<p>Nhân viên có thể thuộc nhiều phòng ban. Khi nhân viên tham gia các phòng ban khác, họ cần xác định phòng ban nào là phòng ban chính. Lưu ý rằng khi nhân viên chỉ thuộc một phòng ban, cột primary của họ là <code>&#39;N&#39;</code>.</p>

<p>Viết lời giải để báo cáo tất cả nhân viên cùng phòng ban chính. Với nhân viên chỉ thuộc một phòng ban, báo cáo phòng ban duy nhất đó.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Employee table:
+-------------+---------------+--------------+
| employee_id | department_id | primary_flag |
+-------------+---------------+--------------+
| 1           | 1             | N            |
| 2           | 1             | Y            |
| 2           | 2             | N            |
| 3           | 3             | N            |
| 4           | 2             | N            |
| 4           | 3             | Y            |
| 4           | 4             | N            |
+-------------+---------------+--------------+
<strong>Đầu ra:</strong>
+-------------+---------------+
| employee_id | department_id |
+-------------+---------------+
| 1           | 1             |
| 2           | 1             |
| 3           | 3             |
| 4           | 3             |
+-------------+---------------+
<strong>Giải thích:</strong>
- Phòng ban chính của nhân viên 1 là 1.
- Phòng ban chính của nhân viên 2 là 1.
- Phòng ban chính của nhân viên 3 là 3.
- Phòng ban chính của nhân viên 4 là 3.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gộp kết quả

<!-- thinking:start -->

> **Tư duy**
>
> Nếu cờ phòng ban chính được bật, chọn hàng đó; nhân viên chỉ thuộc một phòng ban vẫn được báo cáo ngay cả khi cờ là $N$. Dùng Union cho hai tập này.
>
> Chọn các hàng có $\textit{primary\_flag}='Y'$, sau đó $\mathrm{UNION}$ với những nhân viên có số lượng hàng trong nhóm bằng $1$.

<!-- thinking:end -->

Trước tiên, ta truy vấn tất cả nhân viên đã có phòng ban chính, sau đó truy vấn tất cả nhân viên chỉ thuộc một phòng ban. Cuối cùng, ta gộp hai kết quả bằng `UNION`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT employee_id, department_id
FROM Employee
WHERE primary_flag = 'Y'
UNION
SELECT employee_id, department_id
FROM Employee
GROUP BY 1
HAVING COUNT(1) = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
