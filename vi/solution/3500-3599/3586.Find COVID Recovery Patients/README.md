---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3586. Find COVID Recovery Patients](https://leetcode.com/problems/find-covid-recovery-patients)

[中文文档](/solution/3500-3599/3586.Find%20COVID%20Recovery%20Patients/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>patients</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| patient_id  | int     |
| patient_name| varchar |
| age         | int     |
+-------------+---------+
patient_id là mã định danh duy nhất của bảng này.
Mỗi hàng chứa thông tin về một bệnh nhân.
</pre>

<p>Bảng: <code>covid_tests</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| test_id     | int     |
| patient_id  | int     |
| test_date   | date    |
| result      | varchar |
+-------------+---------+
test_id là mã định danh duy nhất của bảng này.
Mỗi hàng biểu thị kết quả một lần xét nghiệm COVID. Kết quả có thể là Positive, Negative hoặc Inconclusive.
</pre>

<p>Hãy viết lời giải để tìm những bệnh nhân đã <strong>hồi phục sau COVID</strong> - những bệnh nhân có kết quả xét nghiệm dương tính nhưng sau đó có kết quả âm tính.</p>

<ul>
    <li>Một bệnh nhân được xem là đã hồi phục nếu có <strong>ít nhất một</strong> lần xét nghiệm <strong>Positive</strong> và sau đó có ít nhất một lần xét nghiệm <strong>Negative</strong> vào <strong>ngày muộn hơn</strong></li>
    <li>Tính <strong>thời gian hồi phục</strong> theo số ngày, là <strong>hiệu</strong> giữa <strong>lần xét nghiệm Positive đầu tiên</strong> và <strong>lần xét nghiệm Negative đầu tiên</strong> sau <strong>lần xét nghiệm Positive đó</strong></li>
    <li><strong>Chỉ đưa vào kết quả</strong> những bệnh nhân có cả kết quả xét nghiệm Positive và Negative</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo </em><code>recovery_time</code><em> theo thứ tự <strong>tăng dần</strong>, sau đó theo </em><code>patient_name</code><em> theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng patients:</p>

<pre class="example-io">
+------------+--------------+-----+
| patient_id | patient_name | age |
+------------+--------------+-----+
| 1          | Alice Smith  | 28  |
| 2          | Bob Johnson  | 35  |
| 3          | Carol Davis  | 42  |
| 4          | David Wilson | 31  |
| 5          | Emma Brown   | 29  |
+------------+--------------+-----+
</pre>

<p>bảng covid_tests:</p>

<pre class="example-io">
+---------+------------+------------+--------------+
| test_id | patient_id | test_date  | result       |
+---------+------------+------------+--------------+
| 1       | 1          | 2023-01-15 | Positive     |
| 2       | 1          | 2023-01-25 | Negative     |
| 3       | 2          | 2023-02-01 | Positive     |
| 4       | 2          | 2023-02-05 | Inconclusive |
| 5       | 2          | 2023-02-12 | Negative     |
| 6       | 3          | 2023-01-20 | Negative     |
| 7       | 3          | 2023-02-10 | Positive     |
| 8       | 3          | 2023-02-20 | Negative     |
| 9       | 4          | 2023-01-10 | Positive     |
| 10      | 4          | 2023-01-18 | Positive     |
| 11      | 5          | 2023-02-15 | Negative     |
| 12      | 5          | 2023-02-20 | Negative     |
+---------+------------+------------+--------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------+--------------+-----+---------------+
| patient_id | patient_name | age | recovery_time |
+------------+--------------+-----+---------------+
| 1          | Alice Smith  | 28  | 10            |
| 3          | Carol Davis  | 42  | 10            |
| 2          | Bob Johnson  | 35  | 11            |
+------------+--------------+-----+---------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Alice Smith (patient_id = 1):</strong>

    <ul>
        <li>Lần xét nghiệm Positive đầu tiên: 2023-01-15</li>
        <li>Lần xét nghiệm Negative đầu tiên sau Positive: 2023-01-25</li>
        <li>Thời gian hồi phục: 25 - 15 = 10 ngày</li>
    </ul>
    </li>
    <li><strong>Bob Johnson (patient_id = 2):</strong>
    <ul>
        <li>Lần xét nghiệm Positive đầu tiên: 2023-02-01</li>
        <li>Lần xét nghiệm Inconclusive vào ngày 2023-02-05 (bỏ qua khi tính thời gian hồi phục)</li>
        <li>Lần xét nghiệm Negative đầu tiên sau Positive: 2023-02-12</li>
        <li>Thời gian hồi phục: 12 - 1 = 11 ngày</li>
    </ul>
    </li>
    <li><strong>Carol Davis (patient_id = 3):</strong>
    <ul>
        <li>Có lần xét nghiệm Negative vào ngày 2023-01-20 (trước lần xét nghiệm Positive)</li>
        <li>Lần xét nghiệm Positive đầu tiên: 2023-02-10</li>
        <li>Lần xét nghiệm Negative đầu tiên sau Positive: 2023-02-20</li>
        <li>Thời gian hồi phục: 20 - 10 = 10 ngày</li>
    </ul>
    </li>
    <li><strong>Các bệnh nhân không được đưa vào kết quả:</strong>
    <ul>
        <li>David Wilson (patient_id = 4): Chỉ có các lần xét nghiệm Positive, không có lần xét nghiệm Negative nào sau Positive</li>
        <li>Emma Brown (patient_id = 5): Chỉ có các lần xét nghiệm Negative, chưa từng có kết quả Positive</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo recovery_time tăng dần, sau đó theo patient_name tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thống kê theo nhóm + Equi-join

<!-- thinking:start -->

> **Tư duy**
>
> Thời gian hồi phục là số ngày từ lần xét nghiệm Positive đầu tiên đến lần xét nghiệm Negative đầu tiên sau đó. Ta lấy ngày Positive nhỏ nhất của mỗi bệnh nhân, sau đó lấy ngày Negative nhỏ nhất xảy ra sau ngày đó.
>
> Inner join hai ngày này, tính hiệu ngày, nối với thông tin bệnh nhân rồi sắp xếp theo thời gian hồi phục và tên bệnh nhân.

<!-- thinking:end -->

Trước tiên, ta tìm ngày xét nghiệm Positive đầu tiên của mỗi bệnh nhân và lưu vào bảng first_positive. Tiếp theo, ta tìm ngày xét nghiệm Negative đầu tiên của mỗi bệnh nhân sau lần xét nghiệm Positive đầu tiên trong bảng covid_tests và lưu vào bảng first_negative_after_positive. Cuối cùng, ta nối hai bảng này với bảng patients, tính thời gian hồi phục rồi sắp xếp theo yêu cầu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    first_positive AS (
        SELECT
            patient_id,
            MIN(test_date) AS first_positive_date
        FROM covid_tests
        WHERE result = 'Positive'
        GROUP BY patient_id
    ),
    first_negative_after_positive AS (
        SELECT
            t.patient_id,
            MIN(t.test_date) AS first_negative_date
        FROM
            covid_tests t
            JOIN first_positive p
                ON t.patient_id = p.patient_id AND t.test_date > p.first_positive_date
        WHERE t.result = 'Negative'
        GROUP BY t.patient_id
    )
SELECT
    p.patient_id,
    p.patient_name,
    p.age,
    DATEDIFF(n.first_negative_date, f.first_positive_date) AS recovery_time
FROM
    first_positive f
    JOIN first_negative_after_positive n ON f.patient_id = n.patient_id
    JOIN patients p ON p.patient_id = f.patient_id
ORDER BY recovery_time ASC, patient_name ASC;
```

#### Pandas

```python
import pandas as pd


def find_covid_recovery_patients(
    patients: pd.DataFrame, covid_tests: pd.DataFrame
) -> pd.DataFrame:
    covid_tests["test_date"] = pd.to_datetime(covid_tests["test_date"])

    pos = (
        covid_tests[covid_tests["result"] == "Positive"]
        .groupby("patient_id", as_index=False)["test_date"]
        .min()
    )
    pos.rename(columns={"test_date": "first_positive_date"}, inplace=True)

    neg = covid_tests.merge(pos, on="patient_id")
    neg = neg[
        (neg["result"] == "Negative") & (neg["test_date"] > neg["first_positive_date"])
    ]
    neg = neg.groupby("patient_id", as_index=False)["test_date"].min()
    neg.rename(columns={"test_date": "first_negative_date"}, inplace=True)

    df = pos.merge(neg, on="patient_id")
    df["recovery_time"] = (
        df["first_negative_date"] - df["first_positive_date"]
    ).dt.days

    out = df.merge(patients, on="patient_id")[
        ["patient_id", "patient_name", "age", "recovery_time"]
    ]
    return out.sort_values(by=["recovery_time", "patient_name"]).reset_index(drop=True)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
