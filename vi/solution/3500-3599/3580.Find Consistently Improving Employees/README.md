---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3580. Find Consistently Improving Employees](https://leetcode.com/problems/find-consistently-improving-employees)

[中文文档](/solution/3500-3599/3580.Find%20Consistently%20Improving%20Employees/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>employees</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| name        | varchar |
+-------------+---------+
employee_id là mã định danh duy nhất của bảng này.
Mỗi hàng chứa thông tin về một nhân viên.
</pre>

<p>Bảng: <code>performance_reviews</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| review_id   | int  |
| employee_id | int  |
| review_date | date |
| rating      | int  |
+-------------+------+
review_id là mã định danh duy nhất của bảng này.
Mỗi hàng biểu thị một lần đánh giá hiệu suất của một nhân viên. rating được chấm theo thang điểm từ 1-5, trong đó 5 là xuất sắc và 1 là kém.
</pre>

<p>Hãy viết lời giải để tìm những nhân viên có hiệu suất được cải thiện đều đặn trong <strong>3 lần đánh giá gần nhất</strong>.</p>

<ul>
	<li>Một nhân viên phải có <strong>ít nhất </strong><code>3</code><strong> lần đánh giá</strong> mới được xét</li>
	<li><strong><code>3</code></strong><strong> lần đánh giá gần nhất</strong> của nhân viên phải có <strong>rating tăng nghiêm ngặt</strong> (mỗi lần đánh giá đều tốt hơn lần trước)</li>
	<li>Sử dụng <code>3</code> lần đánh giá gần nhất dựa trên <code>review_date</code> của mỗi nhân viên</li>
	<li>Tính <strong>improvement score</strong> là hiệu giữa rating mới nhất và rating sớm nhất trong <code>3</code> lần đánh giá gần nhất</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo <strong>improvement score</strong> theo thứ tự <strong>giảm dần</strong>, sau đó theo <strong>name</strong> theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng employees:</p>

<pre class="example-io">
+-------------+----------------+
| employee_id | name           |
+-------------+----------------+
| 1           | Alice Johnson  |
| 2           | Bob Smith      |
| 3           | Carol Davis    |
| 4           | David Wilson   |
| 5           | Emma Brown     |
+-------------+----------------+
</pre>

<p>bảng performance_reviews:</p>

<pre class="example-io">
+-----------+-------------+-------------+--------+
| review_id | employee_id | review_date | rating |
+-----------+-------------+-------------+--------+
| 1         | 1           | 2023-01-15  | 2      |
| 2         | 1           | 2023-04-15  | 3      |
| 3         | 1           | 2023-07-15  | 4      |
| 4         | 1           | 2023-10-15  | 5      |
| 5         | 2           | 2023-02-01  | 3      |
| 6         | 2           | 2023-05-01  | 2      |
| 7         | 2           | 2023-08-01  | 4      |
| 8         | 2           | 2023-11-01  | 5      |
| 9         | 3           | 2023-03-10  | 1      |
| 10        | 3           | 2023-06-10  | 2      |
| 11        | 3           | 2023-09-10  | 3      |
| 12        | 3           | 2023-12-10  | 4      |
| 13        | 4           | 2023-01-20  | 4      |
| 14        | 4           | 2023-04-20  | 4      |
| 15        | 4           | 2023-07-20  | 4      |
| 16        | 5           | 2023-02-15  | 3      |
| 17        | 5           | 2023-05-15  | 2      |
+-----------+-------------+-------------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+----------------+-------------------+
| employee_id | name           | improvement_score |
+-------------+----------------+-------------------+
| 2           | Bob Smith      | 3                 |
| 1           | Alice Johnson  | 2                 |
| 3           | Carol Davis    | 2                 |
+-------------+----------------+-------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Alice Johnson (employee_id = 1):</strong>

    <ul>
        <li>Có 4 lần đánh giá với các rating: 2, 3, 4, 5</li>
        <li>3 lần đánh giá gần nhất (theo ngày): 2023-04-15 (3), 2023-07-15 (4), 2023-10-15 (5)</li>
        <li>Rating tăng nghiêm ngặt: 3 &rarr; 4 &rarr; 5</li>
        <li>Improvement score: 5 - 3 = 2</li>
    </ul>
    </li>
    <li><strong>Carol Davis (employee_id = 3):</strong>
    <ul>
        <li>Có 4 lần đánh giá với các rating: 1, 2, 3, 4</li>
        <li>3 lần đánh giá gần nhất (theo ngày): 2023-06-10 (2), 2023-09-10 (3), 2023-12-10 (4)</li>
        <li>Rating tăng nghiêm ngặt: 2 &rarr; 3 &rarr; 4</li>
        <li>Improvement score: 4 - 2 = 2</li>
    </ul>
    </li>
    <li><strong>Bob Smith (employee_id = 2):</strong>
    <ul>
        <li>Có 4 lần đánh giá với các rating: 3, 2, 4, 5</li>
        <li>3 lần đánh giá gần nhất (theo ngày): 2023-05-01 (2), 2023-08-01 (4), 2023-11-01 (5)</li>
        <li>Rating tăng nghiêm ngặt: 2 &rarr; 4 &rarr; 5</li>
        <li>Improvement score: 5 - 2 = 3</li>
    </ul>
    </li>
    <li><strong>Các nhân viên không được đưa vào kết quả:</strong>
    <ul>
        <li>David Wilson (employee_id = 4): 3 lần đánh giá gần nhất đều là 4 (không có cải thiện)</li>
        <li>Emma Brown (employee_id = 5): Chỉ có 2 lần đánh giá (cần ít nhất 3 lần)</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo improvement_score giảm dần, sau đó theo name tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng hàm cửa sổ và hàm tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ quan tâm liệu ba rating cuối cùng của một nhân viên có tăng nghiêm ngặt hay không; điểm số là rating cuối trừ rating đầu tiên. Đánh số các lần đánh giá của từng nhân viên theo ngày giảm dần rồi dùng phép dịch chuyển để lấy chênh lệch giữa các lần đánh giá liền kề.
>
> Giữ lại các hạng $2$ và $3$ (hai chênh lệch so với lần đánh giá mới nhất), yêu cầu đúng hai hàng và chênh lệch nhỏ nhất dương, nối với tên nhân viên rồi sắp xếp theo điểm số và tên.

<!-- thinking:end -->

Trước tiên, ta lấy ba bản ghi đánh giá hiệu suất gần nhất của mỗi nhân viên và tính chênh lệch rating giữa mỗi lần đánh giá với lần trước đó. Tiếp theo, ta lọc những nhân viên có rating tăng nghiêm ngặt, rồi tính improvement score của họ (tức rating cuối trừ rating đầu tiên trong ba lần đánh giá gần nhất). Cuối cùng, ta sắp xếp kết quả theo improvement score giảm dần và name tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
WITH
    recent AS (
        SELECT
            employee_id,
            review_date,
            ROW_NUMBER() OVER (
                PARTITION BY employee_id
                ORDER BY review_date DESC
            ) AS rn,
            (
                LAG(rating) OVER (
                    PARTITION BY employee_id
                    ORDER BY review_date DESC
                ) - rating
            ) AS delta
        FROM performance_reviews
    )
SELECT
    employee_id,
    name,
    SUM(delta) AS improvement_score
FROM
    recent
    JOIN employees USING (employee_id)
WHERE rn > 1 AND rn <= 3
GROUP BY 1
HAVING COUNT(*) = 2 AND MIN(delta) > 0
ORDER BY 3 DESC, 2;
```

#### Pandas

```python
import pandas as pd


def find_consistently_improving_employees(
    employees: pd.DataFrame, performance_reviews: pd.DataFrame
) -> pd.DataFrame:
    performance_reviews = performance_reviews.sort_values(
        ["employee_id", "review_date"], ascending=[True, False]
    )
    performance_reviews["rn"] = (
        performance_reviews.groupby("employee_id").cumcount() + 1
    )
    performance_reviews["lag_rating"] = performance_reviews.groupby("employee_id")[
        "rating"
    ].shift(1)
    performance_reviews["delta"] = (
        performance_reviews["lag_rating"] - performance_reviews["rating"]
    )
    recent = performance_reviews[
        (performance_reviews["rn"] > 1) & (performance_reviews["rn"] <= 3)
    ]
    improvement = (
        recent.groupby("employee_id")
        .agg(
            improvement_score=("delta", "sum"),
            count=("delta", "count"),
            min_delta=("delta", "min"),
        )
        .reset_index()
    )
    improvement = improvement[
        (improvement["count"] == 2) & (improvement["min_delta"] > 0)
    ]
    result = improvement.merge(employees[["employee_id", "name"]], on="employee_id")
    result = result.sort_values(
        by=["improvement_score", "name"], ascending=[False, True]
    )
    return result[["employee_id", "name", "improvement_score"]]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
