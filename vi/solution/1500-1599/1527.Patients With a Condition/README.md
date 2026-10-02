---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1527. Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition)

[中文文档](/solution/1500-1599/1527.Patients%20With%20a%20Condition/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Patients</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| patient_id   | int     |
| patient_name | varchar |
| conditions   | varchar |
+--------------+---------+
patient_id is the primary key (column with unique values) for this table.
&#39;conditions&#39; contains 0 or more code separated by spaces. 
This table contains information of the patients in the hospital.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm patient_id, patient_name và conditions của các bệnh nhân mắc bệnh tiểu đường Type I. Type I Diabetes luôn bắt đầu bằng tiền tố <code>DIAB1</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>The&nbsp;result format is in the following example.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Patients table:
+------------+--------------+--------------+
| patient_id | patient_name | conditions   |
+------------+--------------+--------------+
| 1          | Daniel       | YFEV COUGH   |
| 2          | Alice        |              |
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 |
| 5          | Alain        | DIAB201      |
+------------+--------------+--------------+
<strong>Output:</strong> 
+------------+--------------+--------------+
| patient_id | patient_name | conditions   |
+------------+--------------+--------------+
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 | 
+------------+--------------+--------------+
<strong>Explanation:</strong> Bob and George both have a condition that starts with DIAB1.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần các bệnh nhân có danh sách condition chứa mã với tiền tố $DIAB1$. Các mã được phân tách bằng dấu cách; dùng riêng $LIKE\ \%DIAB1\%$ cũng sẽ khớp với $DIAB10$.
>
> Mã cần tìm có thể ở đầu ($DIAB1\%$) hoặc sau một dấu cách ($\%\ DIAB1\%$). Kết hợp hai điều kiện tiền tố này sẽ chọn đúng các dòng mà không bắt nhầm mã dài hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    patient_id,
    patient_name,
    conditions
FROM patients
WHERE conditions LIKE 'DIAB1%' OR conditions LIKE '% DIAB1%';
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
