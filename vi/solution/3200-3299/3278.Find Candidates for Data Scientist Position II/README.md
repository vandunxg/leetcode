---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3278. Find Candidates for Data Scientist Position II 🔒](https://leetcode.com/problems/find-candidates-for-data-scientist-position-ii)

[中文文档](/solution/3200-3299/3278.Find%20Candidates%20for%20Data%20Scientist%20Position%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace"><code>Candidates</code></font></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| candidate_id | int     |
| skill        | varchar |
| proficiency  | int     |
+--------------+---------+
(candidate_id, skill) là khóa duy nhất của bảng này.
Mỗi hàng chứa candidate_id, skill và mức độ thành thạo (1-5).
</pre>

<p>Bảng: <font face="monospace"><code>Projects</code></font></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| skill       | varchar |
| importance  | int     |
+-------------+---------+
(project_id, skill) là khóa chính của bảng này.
Mỗi hàng chứa project_id, kỹ năng được yêu cầu và mức độ quan trọng (1-5) của kỹ năng đó đối với dự án.
</pre>

<p>Leetcode đang tuyển nhân sự cho nhiều dự án data science. Hãy viết lời giải để tìm <strong>ứng viên tốt nhất</strong> cho<strong> mỗi dự án</strong> dựa trên các tiêu chí sau:</p>

<ol>
	<li>Ứng viên phải có <strong>tất cả</strong> các kỹ năng mà dự án yêu cầu.</li>
	<li>Tính <strong>điểm</strong> cho mỗi cặp ứng viên-dự án như sau:
	<ul>
		<li><strong>Bắt đầu</strong> với <code>100</code> điểm</li>
		<li><strong>Cộng</strong> <code>10</code> điểm cho mỗi kỹ năng mà <strong>proficiency &gt; importance</strong></li>
		<li><strong>Trừ</strong> <code>5</code> điểm cho mỗi kỹ năng mà <strong>proficiency &lt; importance</strong></li>
		<li>Nếu mức độ thành thạo kỹ năng của ứng viên <strong>bằng </strong> mức độ quan trọng của kỹ năng trong dự án, điểm số không thay đổi</li>
	</ul>
	</li>
</ol>

<p>Chỉ lấy ứng viên đứng đầu (có điểm cao nhất) cho mỗi dự án. Nếu có <strong>hòa</strong>, chọn ứng viên có <code>candidate_id</code> <strong>nhỏ hơn</strong>. Nếu <strong>không có ứng viên phù hợp</strong> cho một dự án, <strong>không trả về</strong>&nbsp;dự án đó.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>project_id</code> theo thứ tự tăng dần.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>Candidates</code>:</p>

<pre class="example-io">
+--------------+-----------+-------------+
| candidate_id | skill     | proficiency |
+--------------+-----------+-------------+
| 101          | Python    | 5           |
| 101          | Tableau   | 3           |
| 101          | PostgreSQL| 4           |
| 101          | TensorFlow| 2           |
| 102          | Python    | 4           |
| 102          | Tableau   | 5           |
| 102          | PostgreSQL| 4           |
| 102          | R         | 4           |
| 103          | Python    | 3           |
| 103          | Tableau   | 5           |
| 103          | PostgreSQL| 5           |
| 103          | Spark     | 4           |
+--------------+-----------+-------------+
</pre>

<p>Bảng <code>Projects</code>:</p>

<pre class="example-io">
+-------------+-----------+------------+
| project_id  | skill     | importance |
+-------------+-----------+------------+
| 501         | Python    | 4          |
| 501         | Tableau   | 3          |
| 501         | PostgreSQL| 5          |
| 502         | Python    | 3          |
| 502         | Tableau   | 4          |
| 502         | R         | 2          |
+-------------+-----------+------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+--------------+-------+
| project_id  | candidate_id | score |
+-------------+--------------+-------+
| 501         | 101          | 105   |
| 502         | 102          | 130   |
+-------------+--------------+-------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với Dự án 501, Ứng viên 101 có điểm cao nhất là 105. Tất cả ứng viên còn lại có cùng điểm, nhưng Ứng viên 101 có candidate_id nhỏ nhất trong số đó.</li>
	<li>Với Dự án 502, Ứng viên 102 có điểm cao nhất là 130.</li>
</ul>

<p>Bảng kết quả được sắp xếp theo project_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + Thống kê nhóm + Hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Một ứng viên phải đáp ứng mọi kỹ năng của dự án; điểm số bắt đầu từ $100$ và được điều chỉnh dựa trên việc proficiency cao hơn hay thấp hơn importance, sau đó mỗi dự án chọn điểm cao nhất rồi đến id nhỏ hơn. Một phép join kết hợp với group có thể cung cấp cả số kỹ năng khớp và điểm số.
>
> Thực hiện equi-join theo skill, tổng hợp số kỹ năng khớp và điểm số, chỉ giữ các hàng có số kỹ năng khớp bằng số kỹ năng của dự án, xếp hạng theo điểm số và id, rồi giữ lại $rk=1$.

<!-- thinking:end -->

Ta có thể thực hiện equi-join giữa bảng `Candidates` và bảng `Projects` trên cột `skill`, đếm số kỹ năng khớp và tính tổng điểm cho mỗi ứng viên trong từng dự án, rồi lưu kết quả vào bảng `S`.

Tiếp theo, ta đếm số kỹ năng được yêu cầu cho từng dự án và lưu kết quả vào bảng `T`.

Sau đó, ta thực hiện equi-join giữa bảng `S` và bảng `T` trên cột `project_id`, lọc các ứng viên có số kỹ năng khớp bằng số kỹ năng được yêu cầu và lưu kết quả vào bảng `P`. Ta tính thứ hạng (`rk`) của mỗi ứng viên trong từng dự án.

Cuối cùng, ta lọc các ứng viên có thứ hạng $rk = 1$ trong mỗi dự án để xác định những ứng viên tốt nhất.

<!-- tabs:start -->

#### MySQL

```sql
WITH
    S AS (
        SELECT
            candidate_id,
            project_id,
            COUNT(*) matched_skills,
            SUM(
                CASE
                    WHEN proficiency > importance THEN 10
                    WHEN proficiency < importance THEN -5
                    ELSE 0
                END
            ) + 100 AS score
        FROM
            Candidates
            JOIN Projects USING (skill)
        GROUP BY 1, 2
    ),
    T AS (
        SELECT project_id, COUNT(1) required_skills
        FROM Projects
        GROUP BY 1
    ),
    P AS (
        SELECT
            project_id,
            candidate_id,
            score,
            RANK() OVER (
                PARTITION BY project_id
                ORDER BY score DESC, candidate_id
            ) rk
        FROM
            S
            JOIN T USING (project_id)
        WHERE matched_skills = required_skills
    )
SELECT project_id, candidate_id, score
FROM P
WHERE rk = 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
