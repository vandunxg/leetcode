---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2854. Rolling Average Steps 🔒](https://leetcode.com/problems/rolling-average-steps)

[中文文档](/solution/2800-2899/2854.Rolling%20Average%20Steps/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Steps</font></code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| steps_count | int  |
| steps_date  | date |
+-------------+------+
(user_id, steps_date) là khóa chính của bảng này.
Mỗi hàng của bảng này chứa user_id, steps_count và steps_date.
</pre>

<p>Hãy viết lời giải để tính <strong>trung bình trượt</strong> <code>3-day</code> của số bước chân cho từng người dùng.</p>

<p>Chúng ta tính <code>n-day</code> <strong>trung bình trượt</strong> như sau:</p>

<ul>
	<li>Với mỗi ngày, nếu có đủ dữ liệu, chúng ta tính trung bình số bước chân của <code>n</code> ngày liên tiếp kết thúc vào ngày đó; nếu không, trung bình trượt <code>n-day</code> không được xác định cho ngày đó.</li>
</ul>

<p>Xuất <code>user_id</code>, <code>steps_date</code> và trung bình trượt. Làm tròn trung bình trượt đến <strong>hai chữ số thập phân</strong>.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo </em><code>user_id</code><em>, </em><code>steps_date</code><em> theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Steps:
+---------+-------------+------------+
| user_id | steps_count | steps_date |
+---------+-------------+------------+
| 1       | 687         | 2021-09-02 |
| 1       | 395         | 2021-09-04 |
| 1       | 499         | 2021-09-05 |
| 1       | 712         | 2021-09-06 |
| 1       | 576         | 2021-09-07 |
| 2       | 153         | 2021-09-06 |
| 2       | 171         | 2021-09-07 |
| 2       | 530         | 2021-09-08 |
| 3       | 945         | 2021-09-04 |
| 3       | 120         | 2021-09-07 |
| 3       | 557         | 2021-09-08 |
| 3       | 840         | 2021-09-09 |
| 3       | 627         | 2021-09-10 |
| 5       | 382         | 2021-09-05 |
| 6       | 480         | 2021-09-01 |
| 6       | 191         | 2021-09-02 |
| 6       | 303         | 2021-09-05 |
+---------+-------------+------------+
<strong>Đầu ra:</strong>
+---------+------------+-----------------+
| user_id | steps_date | rolling_average |
+---------+------------+-----------------+
| 1       | 2021-09-06 | 535.33          |
| 1       | 2021-09-07 | 595.67          |
| 2       | 2021-09-08 | 284.67          |
| 3       | 2021-09-09 | 505.67          |
| 3       | 2021-09-10 | 674.67          |
+---------+------------+-----------------+
<strong>Giải thích:</strong>
- Với user id 1, số bước chân trong ba ngày liên tiếp tính đến 2021-09-06 đều có sẵn. Do đó, trung bình trượt cho ngày này được tính là (395 + 499 + 712) / 3 = 535.33.
- Với user id 1, số bước chân trong ba ngày liên tiếp tính đến 2021-09-07 đều có sẵn. Do đó, trung bình trượt cho ngày này được tính là (499 + 712 + 576) / 3 = 595.67.
- Với user id 2, số bước chân trong ba ngày liên tiếp tính đến 2021-09-08 đều có sẵn. Do đó, trung bình trượt cho ngày này được tính là (153 + 171 + 530) / 3 = 284.67.
- Với user id 3, số bước chân trong ba ngày liên tiếp tính đến 2021-09-09 đều có sẵn. Do đó, trung bình trượt cho ngày này được tính là (120 + 557 + 840) / 3 = 505.67.
- Với user id 3, số bước chân trong ba ngày liên tiếp tính đến 2021-09-10 đều có sẵn. Do đó, trung bình trượt cho ngày này được tính là (557 + 840 + 627) / 3 = 674.67.
- Với user id 4 và 5, không thể tính trung bình trượt vì không đủ dữ liệu của ba ngày liên tiếp. Bảng đầu ra được sắp xếp theo user_id và steps_date theo thứ tự tăng dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Trung bình trượt cần dữ liệu của ba ngày liên tiếp cho cùng một người dùng. Sau khi sắp xếp theo người dùng và ngày, trung bình `ROWS 2 PRECEDING` là trung bình của ba ngày; `LAG(...,2)` cách ngày hiện tại đúng hai ngày sẽ xác nhận tính liên tiếp.

<!-- thinking:end -->

Chúng ta có thể dùng hàm cửa sổ `LAG() OVER()` để tính số ngày chênh lệch giữa ngày hiện tại và ngày trước đó hai ngày của mỗi người dùng. Nếu chênh lệch bằng $2$, điều đó có nghĩa là dữ liệu của $3$ ngày liên tiếp giữa hai ngày này đều tồn tại. Chúng ta có thể dùng hàm cửa sổ `AVG() OVER()` để tính trung bình của $3$ dữ liệu này.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            user_id,
            steps_date,
            ROUND(
                AVG(steps_count) OVER (
                    PARTITION BY user_id
                    ORDER BY steps_date
                    ROWS 2 PRECEDING
                ),
                2
            ) AS rolling_average,
            DATEDIFF(
                steps_date,
                LAG(steps_date, 2) OVER (
                    PARTITION BY user_id
                    ORDER BY steps_date
                )
            ) = 2 AS st
        FROM Steps
    )
SELECT
    user_id,
    steps_date,
    rolling_average
FROM T
WHERE st = 1
ORDER BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
