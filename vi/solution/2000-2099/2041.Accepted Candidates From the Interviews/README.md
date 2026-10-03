---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2041. Accepted Candidates From the Interviews 🔒](https://leetcode.com/problems/accepted-candidates-from-the-interviews)

[中文文档](/solution/2000-2099/2041.Accepted%20Candidates%20From%20the%20Interviews/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Candidates</code></p>

<pre>
+--------------+----------+
| Column Name  | Type     |
+--------------+----------+
| candidate_id | int      |
| name         | varchar  |
| years_of_exp | int      |
| interview_id | int      |
+--------------+----------+
candidate_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này cho biết tên của một ứng viên, số năm kinh nghiệm và ID cuộc phỏng vấn của họ.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Rounds</code></p>

<pre>
+--------------+------+
| Column Name  | Type |
+--------------+------+
| interview_id | int  |
| round_id     | int  |
| score        | int  |
+--------------+------+
(interview_id, round_id) là khóa chính (tổ hợp các cột có các giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này cho biết điểm số của một vòng phỏng vấn.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm ID của những ứng viên có <strong>ít nhất hai</strong> năm kinh nghiệm và tổng điểm của các vòng phỏng vấn của họ <strong>lớn hơn <code>15</code></strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Candidates:
+--------------+---------+--------------+--------------+
| candidate_id | name    | years_of_exp | interview_id |
+--------------+---------+--------------+--------------+
| 11           | Atticus | 1            | 101          |
| 9            | Ruben   | 6            | 104          |
| 6            | Aliza   | 10           | 109          |
| 8            | Alfredo | 0            | 107          |
+--------------+---------+--------------+--------------+
Bảng Rounds:
+--------------+----------+-------+
| interview_id | round_id | score |
+--------------+----------+-------+
| 109          | 3        | 4     |
| 101          | 2        | 8     |
| 109          | 4        | 1     |
| 107          | 1        | 3     |
| 104          | 3        | 6     |
| 109          | 1        | 4     |
| 104          | 4        | 7     |
| 104          | 1        | 2     |
| 109          | 2        | 1     |
| 104          | 2        | 7     |
| 107          | 2        | 3     |
| 101          | 1        | 8     |
+--------------+----------+-------+
<strong>Đầu ra:</strong>
+--------------+
| candidate_id |
+--------------+
| 9            |
+--------------+
<strong>Giải thích:</strong>
- Ứng viên 11: Tổng điểm là 16 và có một năm kinh nghiệm. Không đưa ứng viên này vào bảng kết quả vì số năm kinh nghiệm không đủ.
- Ứng viên 9: Tổng điểm là 22 và có sáu năm kinh nghiệm. Đưa ứng viên này vào bảng kết quả.
- Ứng viên 6: Tổng điểm là 10 và có mười năm kinh nghiệm. Không đưa ứng viên này vào bảng kết quả vì tổng điểm chưa đủ cao.
- Ứng viên 8: Tổng điểm là 6 và không có năm kinh nghiệm. Không đưa ứng viên này vào bảng kết quả vì không đủ số năm kinh nghiệm và tổng điểm.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nối bảng + Nhóm + Lọc

<!-- thinking:start -->

> **Tư duy**
>
> Ứng viên được tuyển cần có ít nhất hai năm kinh nghiệm và tổng điểm phỏng vấn lớn hơn $15$. Các ứng viên được nối với các vòng phỏng vấn dựa trên `interview_id`.
>
> Lọc theo kinh nghiệm, nhóm theo `candidate_id`, rồi dùng `HAVING` để giữ các nhóm có tổng điểm $>15$. Với pandas, ta cũng thực hiện phép tổng hợp tương tự.

<!-- thinking:end -->

Ta có thể nối bảng `Candidates` và bảng `Rounds` dựa trên `interview_id`, lọc các ứng viên có ít nhất 2 năm kinh nghiệm, sau đó nhóm theo `candidate_id` để tính tổng điểm của mỗi ứng viên, cuối cùng lọc các ứng viên có tổng điểm lớn hơn 15.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT candidate_id
FROM
    Candidates
    JOIN Rounds USING (interview_id)
WHERE years_of_exp >= 2
GROUP BY 1
HAVING SUM(score) > 15;
```

#### Pandas

```python
import pandas as pd


def accepted_candidates(candidates: pd.DataFrame, rounds: pd.DataFrame) -> pd.DataFrame:
    merged_df = pd.merge(candidates, rounds, on="interview_id")
    filtered_df = merged_df[merged_df["years_of_exp"] >= 2]
    grouped_df = filtered_df.groupby("candidate_id").agg({"score": "sum"})
    return grouped_df[grouped_df["score"] > 15].reset_index()[["candidate_id"]]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
