---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3182. Find Top Scoring Students 🔒](https://leetcode.com/problems/find-top-scoring-students)

[中文文档](/solution/3100-3199/3182.Find%20Top%20Scoring%20Students/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>students</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| student_id  | int      |
| name        | varchar  |
| major       | varchar  |
+-------------+----------+
student_id là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này chứa ID sinh viên, tên sinh viên và ngành học của họ.
</pre>

<p>Bảng: <code>courses</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| course_id   | int      |
| name        | varchar  |
| credits     | int      |
| major       | varchar  |
+-------------+----------+
course_id là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này chứa ID môn học, tên môn học, số tín chỉ của môn học và ngành mà môn học đó thuộc về.
</pre>

<p>Bảng: <code>enrollments</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| student_id  | int      |
| course_id   | int      |
| semester    | varchar  |
| grade       | varchar  |
+-------------+----------+
(student_id, course_id, semester) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này chứa ID sinh viên, ID môn học, học kỳ và điểm đạt được.
</pre>

<p>Hãy viết lời giải để tìm những sinh viên đã <strong>học</strong> <strong>tất cả các môn học</strong> được cung cấp trong <code>major</code> của họ và đạt <strong>điểm A</strong> ở <strong>tất cả các môn học này</strong>.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>student_id</code> <em>theo <strong>thứ tự tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng students:</p>

<pre class="example-io">
+------------+------------------+------------------+
| student_id | name             | major            |
+------------+------------------+------------------+
| 1          | Alice            | Computer Science |
| 2          | Bob              | Computer Science |
| 3          | Charlie          | Mathematics      |
| 4          | David            | Mathematics      |
+------------+------------------+------------------+
</pre>

<p>bảng courses:</p>

<pre class="example-io">
+-----------+-----------------+---------+------------------+
| course_id | name            | credits | major            |
+-----------+-----------------+---------+------------------+
| 101       | Algorithms      | 3       | Computer Science |
| 102       | Data Structures | 3       | Computer Science |
| 103       | Calculus        | 4       | Mathematics      |
| 104       | Linear Algebra  | 4       | Mathematics      |
+-----------+-----------------+---------+------------------+
</pre>

<p>bảng enrollments:</p>

<pre class="example-io">
+------------+-----------+----------+-------+
| student_id | course_id | semester | grade |
+------------+-----------+----------+-------+
| 1          | 101       | Fall 2023| A     |
| 1          | 102       | Fall 2023| A     |
| 2          | 101       | Fall 2023| B     |
| 2          | 102       | Fall 2023| A     |
| 3          | 103       | Fall 2023| A     |
| 3          | 104       | Fall 2023| A     |
| 4          | 103       | Fall 2023| A     |
| 4          | 104       | Fall 2023| B     |
+------------+-----------+----------+-------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------+
| student_id |
+------------+
| 1          |
| 3          |
+------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Alice (student_id 1) học ngành Computer Science và đã học cả hai môn &quot;Algorithms&quot; và &quot;Data Structures&quot;, đồng thời đạt &#39;A&#39; ở cả hai môn.</li>
	<li>Bob (student_id 2) học ngành Computer Science nhưng không đạt &#39;A&#39; ở tất cả các môn bắt buộc.</li>
	<li>Charlie (student_id 3) học ngành Mathematics và đã học cả hai môn &quot;Calculus&quot; và &quot;Linear Algebra&quot;, đồng thời đạt &#39;A&#39; ở cả hai môn.</li>
	<li>David (student_id 4) học ngành Mathematics nhưng không đạt &#39;A&#39; ở tất cả các môn bắt buộc.</li>
</ul>

<p><b>Lưu ý:</b> Bảng kết quả được sắp xếp theo student_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Join bảng + Grouping

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm những sinh viên đạt $A$ ở mọi môn học thuộc ngành của mình. Nếu kiểm tra riêng “taken” và “got A” thì có thể bỏ sót các lượt đăng ký còn thiếu.
>
> Dùng inner join để kết hợp sinh viên với các môn học thuộc ngành, sau đó left join với enrollments để môn học chưa đăng ký có grade null.
>
> Nhóm theo sinh viên và yêu cầu số điểm $A$s bằng số môn học trong ngành. Phép left join giúp `COUNT` nhận biết các môn còn thiếu, điều mà `SUM(grade='A')` không làm được.

<!-- thinking:end -->

Chúng ta có thể join bảng `students` và bảng `courses` dựa trên trường `major`, sau đó left join bảng `enrollments` với bảng kết quả, cuối cùng group theo `student_id` để lọc ra những sinh viên thỏa mãn các điều kiện.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT student_id
FROM
    students
    JOIN courses USING (major)
    LEFT JOIN enrollments USING (student_id, course_id)
GROUP BY 1
HAVING SUM(grade = 'A') = COUNT(major)
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
