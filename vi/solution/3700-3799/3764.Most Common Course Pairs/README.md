---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3764. Most Common Course Pairs](https://leetcode.com/problems/most-common-course-pairs)

[中文文档](/solution/3700-3799/3764.Most%20Common%20Course%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>course_completions</code></p>

<pre>
+-------------------+---------+
| Column Name       | Type    |
+-------------------+---------+
| user_id           | int     |
| course_id         | int     |
| course_name       | varchar |
| completion_date   | date    |
| course_rating     | int     |
+-------------------+---------+
(user_id, course_id) is the combination of columns with unique values for this table.
Each row represents a completed course by a user with their rating (1-5 scale).
</pre>

<p>Viết lời giải để xác định <strong>lộ trình thành thạo kỹ năng</strong> bằng cách phân tích chuỗi hoàn thành khóa học của những học viên có thành tích cao:</p>

<ul>
    <li>Chỉ xét <strong>những học viên có thành tích cao</strong> (những người đã hoàn thành <strong>ít nhất </strong><code>5</code><strong> khóa học</strong> với <strong>điểm đánh giá trung bình </strong><code>4</code><strong> trở lên</strong>).</li>
    <li>Với mỗi học viên có thành tích cao, xác định <strong>chuỗi khóa học</strong> mà họ đã hoàn thành theo thứ tự thời gian.</li>
    <li>Tìm tất cả <strong>các cặp khóa học liên tiếp</strong> (<code>Course A &rarr; Course B</code>) được những học viên này học.</li>
    <li>Trả về <strong>tần suất của từng cặp</strong>, qua đó xác định những chuyển tiếp khóa học phổ biến nhất trong nhóm học viên có thành tích cao.</li>
</ul>

<p>Trả về <em>bảng kết quả theo</em> <em>tần suất của cặp theo thứ tự <strong>giảm dần</strong></em>&nbsp;<em>, sau đó theo tên khóa học đầu tiên và tên khóa học thứ hai theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng course_completions:</p>

<pre class="example-io">
+---------+-----------+------------------+-----------------+---------------+
| user_id | course_id | course_name      | completion_date | course_rating |
+---------+-----------+------------------+-----------------+---------------+
| 1       | 101       | Python Basics    | 2024-01-05      | 5             |
| 1       | 102       | SQL Fundamentals | 2024-02-10      | 4             |
| 1       | 103       | JavaScript       | 2024-03-15      | 5             |
| 1       | 104       | React Basics     | 2024-04-20      | 4             |
| 1       | 105       | Node.js          | 2024-05-25      | 5             |
| 1       | 106       | Docker           | 2024-06-30      | 4             |
| 2       | 101       | Python Basics    | 2024-01-08      | 4             |
| 2       | 104       | React Basics     | 2024-02-14      | 5             |
| 2       | 105       | Node.js          | 2024-03-20      | 4             |
| 2       | 106       | Docker           | 2024-04-25      | 5             |
| 2       | 107       | AWS Fundamentals | 2024-05-30      | 4             |
| 3       | 101       | Python Basics    | 2024-01-10      | 3             |
| 3       | 102       | SQL Fundamentals | 2024-02-12      | 3             |
| 3       | 103       | JavaScript       | 2024-03-18      | 3             |
| 3       | 104       | React Basics     | 2024-04-22      | 2             |
| 3       | 105       | Node.js          | 2024-05-28      | 3             |
| 4       | 101       | Python Basics    | 2024-01-12      | 5             |
| 4       | 108       | Data Science     | 2024-02-16      | 5             |
| 4       | 109       | Machine Learning | 2024-03-22      | 5             |
+---------+-----------+------------------+-----------------+---------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------------+------------------+------------------+
| first_course     | second_course    | transition_count |
+------------------+------------------+------------------+
| Node.js          | Docker           | 2                |
| React Basics     | Node.js          | 2                |
| Docker           | AWS Fundamentals | 1                |
| JavaScript       | React Basics     | 1                |
| Python Basics    | React Basics     | 1                |
| Python Basics    | SQL Fundamentals | 1                |
| SQL Fundamentals | JavaScript       | 1                |
+------------------+------------------+------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>User 1</strong>: Đã hoàn thành 6 khóa học với điểm đánh giá trung bình là 4.5 (đủ điều kiện là học viên có thành tích cao)</li>
    <li><strong>User 2</strong>: Đã hoàn thành 5 khóa học với điểm đánh giá trung bình là 4.4 (đủ điều kiện là học viên có thành tích cao)</li>
    <li><strong>User 3</strong>: Đã hoàn thành 5 khóa học nhưng điểm đánh giá trung bình là 2.8 (không đủ điều kiện)</li>
    <li><strong>User 4</strong>: Chỉ hoàn thành 3 khóa học (không đủ điều kiện)</li>
    <li><strong>Các cặp khóa học của học viên có thành tích cao</strong>:
    <ul>
        <li>User 1: Python Basics &rarr; SQL Fundamentals &rarr; JavaScript &rarr; React Basics &rarr; Node.js &rarr; Docker</li>
        <li>User 2: Python Basics &rarr; React Basics &rarr; Node.js &rarr; Docker &rarr; AWS Fundamentals</li>
        <li>Các chuyển tiếp phổ biến nhất: Node.js &rarr; Docker (2 lần), React Basics &rarr; Node.js (2 lần)</li>
    </ul>
    </li>
</ul>

<p>Kết quả được sắp xếp theo transition_count theo thứ tự giảm dần, sau đó theo first_course theo thứ tự tăng dần, rồi theo second_course theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Lộ trình khóa học chỉ tính các lần hoàn thành liên tiếp của những học viên có thành tích cao. Trước tiên, chúng ta giữ lại những học viên đã hoàn thành ít nhất năm khóa học và có điểm đánh giá trung bình ít nhất $4$, sau đó tạo các cặp liền kề từ danh sách của từng học viên theo thứ tự thời gian, cuối cùng tổng hợp và sắp xếp các cặp đó.

<!-- thinking:end -->

Trước tiên, chúng ta lọc ra tất cả học viên có thành tích cao, ký hiệu là `top_students`, tức những học viên đã hoàn thành ít nhất 5 khóa học với điểm đánh giá trung bình ít nhất 4. Sau đó, với mỗi học viên có thành tích cao, chúng ta sắp xếp theo thời gian hoàn thành và tìm tất cả các cặp khóa học liên tiếp, ký hiệu là `course_pairs`. Cuối cùng, chúng ta nhóm và đếm tất cả các cặp khóa học, tính số lần xuất hiện của từng cặp, rồi xuất kết quả theo yêu cầu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    top_students AS (
        SELECT user_id
        FROM course_completions
        GROUP BY user_id
        HAVING COUNT(1) >= 5 AND AVG(course_rating) >= 4
    ),
    course_pairs AS (
        SELECT
            course_name AS first_course,
            LEAD(course_name) OVER (
                PARTITION BY user_id
                ORDER BY completion_date
            ) second_course
        FROM
            top_students
            JOIN course_completions USING (user_id)
    )
SELECT
    *,
    COUNT(1) transition_count
FROM course_pairs
WHERE second_course IS NOT NULL
GROUP BY 1, 2
ORDER BY 3 DESC, 1, 2;
```

#### Pandas

```python
import pandas as pd


def topLearnerCourseTransitions(course_completions: pd.DataFrame) -> pd.DataFrame:
    grp = course_completions.groupby("user_id")
    top_students = grp.filter(
        lambda df: df.shape[0] >= 5 and df["course_rating"].mean() >= 4
    )["user_id"].unique()

    df = course_completions[course_completions["user_id"].isin(top_students)].copy()
    df = df.sort_values(["user_id", "completion_date"])
    df["second_course"] = df.groupby("user_id")["course_name"].shift(-1)
    df["first_course"] = df["course_name"]

    pairs = df[df["second_course"].notna()][["first_course", "second_course"]]

    result = (
        pairs.groupby(["first_course", "second_course"])
        .size()
        .reset_index(name="transition_count")
        .sort_values(
            ["transition_count", "first_course", "second_course"],
            ascending=[False, True, True],
            key=lambda col: col.str.lower() if col.dtype == "object" else col,
        )
        .reset_index(drop=True)
    )

    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
