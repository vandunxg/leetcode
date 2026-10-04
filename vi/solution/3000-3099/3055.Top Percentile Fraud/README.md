---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3055. Top Percentile Fraud 🔒](https://leetcode.com/problems/top-percentile-fraud)

[中文文档](/solution/3000-3099/3055.Top%20Percentile%20Fraud/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Fraud</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| policy_id   | int     |
| state       | varchar |
| fraud_score | int     |
+-------------+---------+
policy_id là cột chứa các giá trị duy nhất trong bảng này.
Bảng này chứa policy id, state và fraud score.
</pre>

<p>Leetcode Insurance Corp đã phát triển một <strong>mô hình dự đoán </strong>dựa trên ML để phát hiện <strong>khả năng</strong> các yêu cầu bồi thường gian lận. Vì vậy, họ phân công những chuyên viên xử lý yêu cầu giàu kinh nghiệm nhất để xử lý <code>5%</code> <strong>yêu cầu bồi thường</strong> <strong>bị gắn cờ</strong> ở mức cao nhất bởi mô hình này.</p>

<p>Hãy viết lời giải để tìm <code>5</code> <strong>phần trăm cao nhất</strong> các yêu cầu bồi thường từ <strong>mỗi bang</strong>.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo </em><code>state</code><em> theo thứ tự <strong>tăng dần</strong>, </em><code>fraud_score</code><em> theo thứ tự <strong>giảm dần</strong>, và </em><code>policy_id</code><em> theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Fraud:
+-----------+------------+-------------+
| policy_id | state      | fraud_score |
+-----------+------------+-------------+
| 1         | California | 0.92        |
| 2         | California | 0.68        |
| 3         | California | 0.17        |
| 4         | New York   | 0.94        |
| 5         | New York   | 0.81        |
| 6         | New York   | 0.77        |
| 7         | Texas      | 0.98        |
| 8         | Texas      | 0.97        |
| 9         | Texas      | 0.96        |
| 10        | Florida    | 0.97        |
| 11        | Florida    | 0.98        |
| 12        | Florida    | 0.78        |
| 13        | Florida    | 0.88        |
| 14        | Florida    | 0.66        |
+-----------+------------+-------------+
<strong>Đầu ra:</strong>
+-----------+------------+-------------+
| policy_id | state      | fraud_score |
+-----------+------------+-------------+
| 1         | California | 0.92        |
| 11        | Florida    | 0.98        |
| 4         | New York   | 0.94        |
| 7         | Texas      | 0.98        |
+-----------+------------+-------------+
<strong>Giải thích</strong>
- Đối với bang California, chỉ policy ID 1, với fraud score 0.92, thuộc 5 phần trăm cao nhất của bang này.
- Đối với bang Florida, chỉ policy ID 11, với fraud score 0.98, thuộc 5 phần trăm cao nhất của bang này.
- Đối với bang New York, chỉ policy ID 4, với fraud score 0.94, thuộc 5 phần trăm cao nhất của bang này.
- Đối với bang Texas, chỉ policy ID 7, với fraud score 0.98, thuộc 5 phần trăm cao nhất của bang này.
Bảng kết quả được sắp xếp theo state tăng dần, fraud score giảm dần và policy ID tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bang giữ lại các policy có fraud score cao nhất, bao gồm cả các policy đồng hạng. Window ranking trực tiếp hơn so với subquery.
>
> $\texttt{RANK}$ phân vùng theo state và sắp xếp theo score giảm dần, từ đó đánh dấu mọi score cao nhất bằng rank $1$.
>
> Giữ lại $\textit{rk}=1$ rồi sắp xếp theo state, score và policy id.

<!-- thinking:end -->

Ta có thể sử dụng hàm cửa sổ `RANK()` để tính thứ hạng của fraud score trong từng state, sau đó lọc các bản ghi có thứ hạng bằng 1 và sắp xếp chúng theo yêu cầu của đề bài.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY state
                ORDER BY fraud_score DESC
            ) AS rk
        FROM Fraud
    )
SELECT policy_id, state, fraud_score
FROM T
WHERE rk = 1
ORDER BY 2, 3 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
