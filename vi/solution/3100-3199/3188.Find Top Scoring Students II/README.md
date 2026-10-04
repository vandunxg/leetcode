---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3188. Find Top Scoring Students II 🔒](https://leetcode.com/problems/find-top-scoring-students-ii)

[中文文档](/solution/3100-3199/3188.Find%20Top%20Scoring%20Students%20II/README.md)

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
student_id là khóa chính của bảng này.
Mỗi dòng chứa ID sinh viên, tên sinh viên và chuyên ngành của họ.
</pre>

<p>Bảng: <code>courses</code></p>

<pre>
+-------------+-------------------+
| Column Name | Type              |
+-------------+-------------------+
| course_id   | int               |
| name        | varchar           |
| credits     | int               |
| major       | varchar           |
| mandatory   | enum              |
+-------------+-------------------+
course_id là khóa chính của bảng này.
mandatory có kiểu enum gồm (&#39;Yes&#39;, &#39;No&#39;).
Mỗi dòng chứa ID khóa học, tên khóa học, số tín chỉ, chuyên ngành mà khóa học thuộc về và khóa học có bắt buộc hay không.
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
| GPA         | decimal  |
+-------------+----------+
(student_id, course_id, semester) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi dòng chứa ID sinh viên, ID khóa học, học kỳ và điểm số nhận được.
</pre>

<p>Hãy viết lời giải để tìm các sinh viên thỏa mãn những tiêu chí sau:</p>

<ul>
	<li><strong>Đã học tất cả các khóa học bắt buộc</strong> và <strong>ít nhất hai</strong> khóa học tự chọn được cung cấp trong <strong>chuyên ngành của họ.</strong></li>
	<li>Đạt điểm <strong>A</strong> trong <strong>tất cả các khóa học bắt buộc</strong> và ít nhất <strong>B</strong> trong <strong>các khóa học tự chọn</strong>.</li>
	<li>Duy trì <code>GPA</code> trung bình ít nhất&nbsp;<code>2.5</code> cho tất cả các khóa học (bao gồm cả các khóa học ngoài chuyên ngành của họ).</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>student_id</code> <em>theo <strong>thứ tự tăng dần</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng students:</p>

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

<p>Bảng courses:</p>

<pre class="example-io">
 +-----------+-------------------+---------+------------------+----------+
 | course_id | name              | credits | major            | mandatory|
 +-----------+-------------------+---------+------------------+----------+
 | 101       | Algorithms        | 3       | Computer Science | yes      |
 | 102       | Data Structures   | 3       | Computer Science | yes      |
 | 103       | Calculus          | 4       | Mathematics      | yes      |
 | 104       | Linear Algebra    | 4       | Mathematics      | yes      |
 | 105       | Machine Learning  | 3       | Computer Science | no       |
 | 106       | Probability       | 3       | Mathematics      | no       |
 | 107       | Operating Systems | 3       | Computer Science | no       |
 | 108       | Statistics        | 3       | Mathematics      | no       |
 +-----------+-------------------+---------+------------------+----------+
</pre>

<p>Bảng enrollments:</p>

<pre class="example-io">
 +------------+-----------+-------------+-------+-----+
 | student_id | course_id | semester    | grade | GPA |
 +------------+-----------+-------------+-------+-----+
 | 1          | 101       | Fall 2023   | A     | 4.0 |
 | 1          | 102       | Spring 2023 | A     | 4.0 |
 | 1          | 105       | Spring 2023 | A     | 4.0 |
 | 1          | 107       | Fall 2023   | B     | 3.5 |
 | 2          | 101       | Fall 2023   | A     | 4.0 |
 | 2          | 102       | Spring 2023 | B     | 3.0 |
 | 3          | 103       | Fall 2023   | A     | 4.0 |
 | 3          | 104       | Spring 2023 | A     | 4.0 |
 | 3          | 106       | Spring 2023 | A     | 4.0 |
 | 3          | 108       | Fall 2023   | B     | 3.5 |
 | 4          | 103       | Fall 2023   | B     | 3.0 |
 | 4          | 104       | Spring 2023 | B     | 3.0 |
 +------------+-----------+-------------+-------+-----+
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
	<li>Alice (student_id 1) học chuyên ngành Computer Science và đã học cả Algorithms và Data Structures, đạt điểm A ở cả hai môn. Cô ấy cũng đã học Machine Learning và Operating Systems như các môn tự chọn, lần lượt đạt điểm A và B.</li>
	<li>Bob (student_id 2) học chuyên ngành Computer Science nhưng không đạt điểm A ở tất cả các khóa học bắt buộc.</li>
	<li>Charlie (student_id 3) học chuyên ngành Mathematics và đã học cả Calculus và Linear Algebra, đạt điểm A ở cả hai môn. Cậu ấy cũng đã học Probability và Statistics như các môn tự chọn, lần lượt đạt điểm A và B.</li>
	<li>David (student_id 4) học chuyên ngành Mathematics nhưng không đạt điểm A ở tất cả các khóa học bắt buộc.</li>
</ul>

<p><strong>Lưu ý:</strong> Bảng kết quả được sắp xếp theo student_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Join + Group + Lọc có điều kiện

<!-- thinking:start -->

> **Tư duy**
>
> Ngoài GPA, mọi khóa học bắt buộc đều phải đạt $A$ và phải có ít nhất hai khóa học tự chọn đạt $A$ hoặc $B$. Nếu tách các điều kiện này thành những truy vấn riêng, chúng ta có thể bỏ sót các lượt đăng ký có giá trị null.
>
> Một CTE giữ lại các sinh viên có GPA $\ge 2.5$, sau đó join các khóa học theo chuyên ngành và left join điểm số để các tổng có điều kiện có thể biểu diễn từng quy tắc.
>
> `HAVING` đánh đồng số điểm $A$ của các khóa học bắt buộc với số khóa học bắt buộc, yêu cầu các khóa học tự chọn phải tồn tại và có điểm thuộc $\{A,B\}$, đồng thời yêu cầu có ít nhất hai khóa học tự chọn.

<!-- thinking:end -->

Đầu tiên, chúng ta lọc các sinh viên có GPA trung bình lớn hơn hoặc bằng 2.5 và lưu họ trong bảng `T`.

Tiếp theo, chúng ta join bảng `T` với bảng `students` dựa trên `student_id`, sau đó join với bảng `courses` dựa trên `major`, và cuối cùng thực hiện left join với bảng `enrollments` dựa trên `student_id` và `course_id`.

Sau đó, chúng ta nhóm theo ID sinh viên, sử dụng mệnh đề `HAVING` để lọc ra những sinh viên thỏa mãn các điều kiện, rồi sắp xếp theo ID sinh viên.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT student_id
        FROM enrollments
        GROUP BY 1
        HAVING AVG(GPA) >= 2.5
    )
SELECT student_id
FROM
    T
    JOIN students USING (student_id)
    JOIN courses USING (major)
    LEFT JOIN enrollments USING (student_id, course_id)
GROUP BY 1
HAVING
    SUM(mandatory = 'yes' AND grade = 'A') = SUM(mandatory = 'yes')
    AND SUM(mandatory = 'no' AND grade IS NOT NULL) = SUM(mandatory = 'no' AND grade IN ('A', 'B'))
    AND SUM(mandatory = 'no' AND grade IS NOT NULL) >= 2
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
