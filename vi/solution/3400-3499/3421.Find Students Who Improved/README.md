---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3421. Find Students Who Improved](https://leetcode.com/problems/find-students-who-improved)

[中文文档](/solution/3400-3499/3421.Find%20Students%20Who%20Improved/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Scores</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student_id  | int     |
| subject     | varchar |
| score       | int     |
| exam_date   | varchar |
+-------------+---------+
(student_id, subject, exam_date) là khóa chính của bảng này.
Mỗi hàng chứa thông tin về điểm số của một học sinh trong một môn học cụ thể vào một ngày thi nhất định. score nằm trong khoảng từ 0 đến 100 (bao gồm cả hai đầu mút).
</pre>

<p>Viết lời giải để tìm <strong>những học sinh có tiến bộ</strong>. Một học sinh được xem là có tiến bộ nếu thỏa mãn <strong>cả hai</strong> điều kiện sau:</p>

<ul>
	<li>Đã thi cùng một <strong>môn học</strong> vào ít nhất hai ngày khác nhau</li>
	<li><strong>Điểm số mới nhất</strong> của môn học đó <strong>cao hơn</strong> <strong>điểm số đầu tiên</strong></li>
</ul>

<p>Trả về <em>bảng kết quả</em>, <em>được sắp xếp theo</em> <code>student_id,</code> <code>subject</code> theo thứ tự <em><strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Scores:</p>

<pre class="example-io">
+------------+----------+-------+------------+
| student_id | subject  | score | exam_date  |
+------------+----------+-------+------------+
| 101        | Math     | 70    | 2023-01-15 |
| 101        | Math     | 85    | 2023-02-15 |
| 101        | Physics  | 65    | 2023-01-15 |
| 101        | Physics  | 60    | 2023-02-15 |
| 102        | Math     | 80    | 2023-01-15 |
| 102        | Math     | 85    | 2023-02-15 |
| 103        | Math     | 90    | 2023-01-15 |
| 104        | Physics  | 75    | 2023-01-15 |
| 104        | Physics  | 85    | 2023-02-15 |
+------------+----------+-------+------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------+----------+-------------+--------------+
| student_id | subject  | first_score | latest_score |
+------------+----------+-------------+--------------+
| 101        | Math     | 70          | 85           |
| 102        | Math     | 80          | 85           |
| 104        | Physics  | 75          | 85           |
+------------+----------+-------------+--------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Học sinh 101 môn Math: Tăng từ 70 lên 85</li>
	<li>Học sinh 101 môn Physics: Không tiến bộ (giảm từ 65 xuống 60)</li>
	<li>Học sinh 102 môn Math: Tăng từ 80 lên 85</li>
	<li>Học sinh 103 môn Math: Chỉ thi một lần, không đủ điều kiện</li>
	<li>Học sinh 104 môn Physics: Tăng từ 75 lên 85</li>
</ul>

<p>Bảng kết quả được sắp xếp theo student_id, subject.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ + Subquery + Lọc có điều kiện

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần tìm những học sinh có điểm số mới nhất trong một môn học cao hơn điểm số đầu tiên. Việc gom nhóm $\textit{MIN}/\textit{MAX}(\textit{exam\_date})$ rồi join ngược lại có thể gắn nhầm điểm số với một ngày thi.
>
> Các hàm cửa sổ đánh dấu cả bài thi sớm nhất và mới nhất chỉ trong một lần duyệt.
>
> Chúng ta tính $\textit{ROW\_NUMBER}()$ được phân vùng theo $(\textit{student\_id},\textit{subject})$ theo cả hai thứ tự ngày thi, self-join hai thứ hạng, giữ lại các hàng có $\textit{latest\_score}>\textit{first\_score}$, rồi sắp xếp theo yêu cầu.

<!-- thinking:end -->

Đầu tiên, chúng ta sử dụng hàm cửa sổ `ROW_NUMBER()` để tính thứ hạng của ngày thi của mỗi học sinh trong từng môn học, lần lượt tính thứ hạng của bài thi đầu tiên và bài thi gần nhất cho mỗi học sinh trong từng môn học.

Sau đó, chúng ta sử dụng phép `JOIN` trong subquery để nối điểm số của bài thi đầu tiên và bài thi gần nhất. Cuối cùng, theo yêu cầu của đề bài, chúng ta lọc ra những học sinh có điểm số ở bài thi gần nhất cao hơn điểm số ở bài thi đầu tiên.

<!-- tabs:start -->

#### MySQL

```sql
WITH
    RankedScores AS (
        SELECT
            student_id,
            subject,
            score,
            exam_date,
            ROW_NUMBER() OVER (
                PARTITION BY student_id, subject
                ORDER BY exam_date ASC
            ) AS rn_first,
            ROW_NUMBER() OVER (
                PARTITION BY student_id, subject
                ORDER BY exam_date DESC
            ) AS rn_latest
        FROM Scores
    ),
    FirstAndLatestScores AS (
        SELECT
            f.student_id,
            f.subject,
            f.score AS first_score,
            l.score AS latest_score
        FROM
            RankedScores f
            JOIN RankedScores l ON f.student_id = l.student_id AND f.subject = l.subject
        WHERE f.rn_first = 1 AND l.rn_latest = 1
    )
SELECT
    *
FROM FirstAndLatestScores
WHERE latest_score > first_score
ORDER BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
