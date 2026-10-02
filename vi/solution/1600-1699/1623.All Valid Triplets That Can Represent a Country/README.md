---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1623. All Valid Triplets That Can Represent a Country 🔒](https://leetcode.com/problems/all-valid-triplets-that-can-represent-a-country)

[中文文档](/solution/1600-1699/1623.All%20Valid%20Triplets%20That%20Can%20Represent%20a%20Country/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>SchoolA</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
student_id is the column with unique values for this table.
Each row of this table contains the name and the id of a student in school A.
All student_name are distinct.
</pre>

<p>&nbsp;</p>

<p>Table: <code>SchoolB</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
student_id is the column with unique values for this table.
Each row of this table contains the name and the id of a student in school B.
All student_name are distinct.
</pre>

<p>&nbsp;</p>

<p>Table: <code>SchoolC</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
student_id is the column with unique values for this table.
Each row of this table contains the name and the id of a student in school C.
All student_name are distinct.
</pre>

<p>&nbsp;</p>

<p>Có một quốc gia với ba trường học, mỗi học sinh theo học tại <strong>đúng một</strong> trường. Quốc gia này tham gia một cuộc thi và muốn chọn một học sinh từ mỗi trường để đại diện, sao cho:</p>

<ul>
	<li><code>member_A</code> được chọn từ <code>SchoolA</code>,</li>
	<li><code>member_B</code> được chọn từ <code>SchoolB</code>,</li>
	<li><code>member_C</code> được chọn từ <code>SchoolC</code>, và</li>
	<li>Tên và ID của các học sinh được chọn đôi một khác nhau (nghĩa là không có hai học sinh nào trùng tên hoặc trùng ID).</li>
</ul>

<p>Viết lời giải để tìm tất cả bộ ba có thể đại diện cho quốc gia theo các ràng buộc trên.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
SchoolA table:
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 1          | Alice        |
| 2          | Bob          |
+------------+--------------+
SchoolB table:
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 3          | Tom          |
+------------+--------------+
SchoolC table:
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 3          | Tom          |
| 2          | Jerry        |
| 10         | Alice        |
+------------+--------------+
<strong>Output:</strong> 
+----------+----------+----------+
| member_A | member_B | member_C |
+----------+----------+----------+
| Alice    | Tom      | Jerry    |
| Bob      | Tom      | Alice    |
+----------+----------+----------+
<strong>Explanation:</strong> 
Hãy xem tất cả các bộ ba có thể có.
- (Alice, Tom, Tom) --&gt; Rejected because member_B and member_C have the same name and the same ID.
- (Alice, Tom, Jerry) --&gt; Valid triplet.
- (Alice, Tom, Alice) --&gt; Rejected because member_A and member_C have the same name.
- (Bob, Tom, Tom) --&gt; Rejected because member_B and member_C have the same name and the same ID.
- (Bob, Tom, Jerry) --&gt; Rejected because member_A and member_C have the same ID.
- (Bob, Tom, Alice) --&gt; Valid triplet.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chọn một học sinh từ mỗi trường sao cho ID và tên đôi một khác nhau. Các bảng nhỏ, nên tích Descartes ba chiều kết hợp với các điều kiện lọc là đủ.
>
> Mệnh đề $\texttt{WHERE}$ yêu cầu cả ba cặp ID và cả ba cặp tên đều khác nhau; các tên được chọn chính là mọi bộ ba hợp lệ.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    a.student_name AS member_A,
    b.student_name AS member_B,
    c.student_name AS member_C
FROM
    SchoolA AS a,
    SchoolB AS b,
    SchoolC AS c
WHERE
    a.student_name != b.student_name
    AND a.student_name != c.student_name
    AND b.student_name != c.student_name
    AND a.student_id != b.student_id
    AND a.student_id != c.student_id
    AND b.student_id != c.student_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
