---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1412. Find the Quiet Students in All Exams 🔒](https://leetcode.com/problems/find-the-quiet-students-in-all-exams)

[中文文档](/solution/1400-1499/1412.Find%20the%20Quiet%20Students%20in%20All%20Exams/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Student</code></p>

<pre>
+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| student_id          | int     |
| student_name        | varchar |
+---------------------+---------+
student_id là khóa chính (cột có các giá trị không trùng lặp) của bảng này.
student_name là tên của học sinh.</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Exam</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| exam_id       | int     |
| student_id    | int     |
| score         | int     |
+---------------+---------+
(exam_id, student_id) là khóa chính (tổ hợp các cột có các giá trị không trùng lặp) của bảng này.
Mỗi hàng trong bảng này cho biết học sinh có student_id đạt một số điểm trong kỳ thi có id là exam_id.
</pre>

<p>&nbsp;</p>

<p>Một <strong>học sinh kín tiếng</strong> là học sinh đã tham gia ít nhất một kỳ thi và không đạt điểm cao nhất hoặc thấp nhất.</p>

<p>Hãy viết lời giải để báo cáo các học sinh <code>(student_id, student_name)</code> luôn kín tiếng trong tất cả các kỳ thi. Không trả về học sinh chưa từng tham gia kỳ thi nào.</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>student_id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Student:
+-------------+---------------+
| student_id  | student_name  |
+-------------+---------------+
| 1           | Daniel        |
| 2           | Jade          |
| 3           | Stella        |
| 4           | Jonathan      |
| 5           | Will          |
+-------------+---------------+
Bảng Exam:
+------------+--------------+-----------+
| exam_id    | student_id   | score     |
+------------+--------------+-----------+
| 10         |     1        |    70     |
| 10         |     2        |    80     |
| 10         |     3        |    90     |
| 20         |     1        |    80     |
| 30         |     1        |    70     |
| 30         |     3        |    80     |
| 30         |     4        |    90     |
| 40         |     1        |    60     |
| 40         |     2        |    70     |
| 40         |     4        |    80     |
+------------+--------------+-----------+
<strong>Đầu ra:</strong>
+-------------+---------------+
| student_id  | student_name  |
+-------------+---------------+
| 2           | Jade          |
+-------------+---------------+
<strong>Giải thích:</strong>
Trong kỳ thi 1: Học sinh 1 và 3 lần lượt đạt điểm thấp nhất và cao nhất.
Trong kỳ thi 2: Học sinh 1 đạt cả điểm cao nhất và thấp nhất.
Trong kỳ thi 3 và 4: Học sinh 1 và 4 lần lượt đạt điểm thấp nhất và cao nhất.
Học sinh 2 và 5 chưa từng đạt điểm cao nhất hoặc thấp nhất trong bất kỳ kỳ thi nào.
Vì học sinh 5 không tham gia kỳ thi nào nên bị loại khỏi kết quả.
Do đó, ta chỉ trả về thông tin của học sinh 2.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng hàm cửa sổ RANK() + Group By

<!-- thinking:start -->

> **Tư duy**
>
> Một học sinh kín tiếng không bao giờ là người duy nhất đạt điểm cao nhất hoặc thấp nhất trong bất kỳ kỳ thi nào mà họ tham gia. Việc xếp hạng đồng thời hai đầu điểm số trong từng kỳ thi sẽ đơn giản hơn so với việc lọc các giá trị cực trị theo từng kỳ thi.
>
> `RANK()` trên từng `exam_id` theo cả hai hướng sẽ đánh dấu điểm thấp nhất và cao nhất. Sau khi join với `Student`, ta giữ lại những học sinh có số lần đạt hạng 1 theo cả hai hướng đều bằng 0.

<!-- thinking:end -->

Ta có thể sử dụng hàm cửa sổ `RANK()` để tính hạng tăng dần $rk1$ và hạng giảm dần $rk2$ của mỗi học sinh trong từng kỳ thi, từ đó thu được bảng $T$.

Tiếp theo, ta có thể thực hiện inner join giữa bảng $T$ và bảng $Student$, sau đó group by mã học sinh để tính số lần mỗi học sinh có hạng 1 theo thứ tự tăng dần $cnt1$ và theo thứ tự giảm dần $cnt2$ trong tất cả các kỳ thi. Nếu cả $cnt1$ và $cnt2$ đều bằng $0$, điều đó có nghĩa là học sinh luôn ở giữa nhóm trong tất cả các kỳ thi.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            student_id,
            RANK() OVER (
                PARTITION BY exam_id
                ORDER BY score
            ) AS rk1,
            RANK() OVER (
                PARTITION BY exam_id
                ORDER BY score DESC
            ) AS rk2
        FROM Exam
    )
SELECT student_id, student_name
FROM
    T
    JOIN Student USING (student_id)
GROUP BY 1
HAVING SUM(rk1 = 1) = 0 AND SUM(rk2 = 1) = 0
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
