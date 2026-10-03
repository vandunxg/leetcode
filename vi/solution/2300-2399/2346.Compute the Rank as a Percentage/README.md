---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2346. Compute the Rank as a Percentage 🔒](https://leetcode.com/problems/compute-the-rank-as-a-percentage)

[中文文档](/solution/2300-2399/2346.Compute%20the%20Rank%20as%20a%20Percentage/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Students</code></p>

<pre>
+---------------+------+
| Column Name   | Type |
+---------------+------+
| student_id    | int  |
| department_id | int  |
| mark          | int  |
+---------------+------+
student_id chứa các giá trị duy nhất.
Mỗi hàng trong bảng này cho biết ID của sinh viên, ID của khoa mà sinh viên theo học và điểm thi của họ.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo thứ hạng của từng sinh viên trong khoa dưới dạng phần trăm, trong đó thứ hạng dưới dạng phần trăm được tính theo công thức: <code>(student_rank_in_the_department - 1) * 100 / (the_number_of_students_in_the_department - 1)</code>. <code>percentage</code> phải được <strong>làm tròn đến 2 chữ số thập phân</strong>. <code>student_rank_in_the_department</code> được xác định theo <strong>thứ tự giảm dần</strong><b> </b>của <code>mark</code>, sao cho sinh viên có <code>mark</code> cao nhất có <code>rank 1</code>. Nếu hai sinh viên có cùng điểm, họ cũng có cùng thứ hạng.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Students:
+------------+---------------+------+
| student_id | department_id | mark |
+------------+---------------+------+
| 2          | 2             | 650  |
| 8          | 2             | 650  |
| 7          | 1             | 920  |
| 1          | 1             | 610  |
| 3          | 1             | 530  |
+------------+---------------+------+
<strong>Đầu ra:</strong>
+------------+---------------+------------+
| student_id | department_id | percentage |
+------------+---------------+------------+
| 7          | 1             | 0.0        |
| 1          | 1             | 50.0       |
| 3          | 1             | 100.0      |
| 2          | 2             | 0.0        |
| 8          | 2             | 0.0        |
+------------+---------------+------------+
<strong>Giải thích:</strong>
Với khoa 1:
 - Sinh viên 7: percentage = (1 - 1) * 100 / (3 - 1) = 0.0
 - Sinh viên 1: percentage = (2 - 1) * 100 / (3 - 1) = 50.0
 - Sinh viên 3: percentage = (3 - 1) * 100 / (3 - 1) = 100.0
Với khoa 2:
 - Sinh viên 2: percentage = (1 - 1) * 100 / (2 - 1) = 0.0
 - Sinh viên 8: percentage = (1 - 1) * 100 / (2 - 1) = 0.0
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phần trăm là $(\textit{rank}-1)$ chia cho $(\textit{size}-1)$. Một khoa chỉ có một sinh viên sẽ có mẫu số bằng không.
>
> Dùng $RANK$ để đánh hạng theo mark giảm dần và dùng window $COUNT$ để đếm số sinh viên trong khoa. $IFNULL$ trả về $0$ cho trường hợp chỉ có một sinh viên; các trường hợp còn lại được làm tròn đến hai chữ số thập phân.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    student_id,
    department_id,
    IFNULL(
        ROUND(
            (
                RANK() OVER (
                    PARTITION BY department_id
                    ORDER BY mark DESC
                ) - 1
            ) * 100 / (COUNT(1) OVER (PARTITION BY department_id) - 1),
            2
        ),
        0
    ) AS percentage
FROM Students;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
