---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2175. The Change in Global Rankings 🔒](https://leetcode.com/problems/the-change-in-global-rankings)

[中文文档](/solution/2100-2199/2175.The%20Change%20in%20Global%20Rankings/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>TeamPoints</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| team_id     | int     |
| name        | varchar |
| points      | int     |
+-------------+---------+
team_id chứa các giá trị duy nhất.
Mỗi hàng của bảng này chứa ID của một đội tuyển quốc gia, tên quốc gia mà đội đại diện và số điểm của đội trên bảng xếp hạng toàn cầu. Không có hai đội nào đại diện cho cùng một quốc gia.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>PointsChange</code></p>

<pre>
+---------------+------+
| Tên cột       | Kiểu |
+---------------+------+
| team_id       | int  |
| points_change | int  |
+---------------+------+
team_id chứa các giá trị duy nhất.
Mỗi hàng của bảng này chứa ID của một đội tuyển quốc gia và mức thay đổi điểm của đội trên bảng xếp hạng toàn cầu.
points_change có thể là:
- 0: cho biết điểm không thay đổi.
- số dương: cho biết điểm tăng.
- số âm: cho biết điểm giảm.
Mỗi team_id xuất hiện trong TeamPoints cũng sẽ xuất hiện trong bảng này.
</pre>

<p>&nbsp;</p>

<p><strong>bảng xếp hạng toàn cầu</strong> của một đội tuyển quốc gia là thứ hạng của đội sau khi sắp xếp tất cả các đội theo điểm số <strong>giảm dần</strong>. Nếu hai đội có cùng số điểm, chúng ta phá vỡ kết quả hòa bằng cách sắp xếp theo tên đội theo <strong>thứ tự từ điển</strong>.</p>

<p>Điểm của mỗi đội tuyển quốc gia cần được cập nhật dựa trên giá trị <code>points_change</code> tương ứng.</p>

<p>Hãy viết lời giải để tính mức thay đổi trong bảng xếp hạng toàn cầu sau khi cập nhật điểm của mỗi đội.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng TeamPoints:
+---------+-------------+--------+
| team_id | name        | points |
+---------+-------------+--------+
| 3       | Algeria     | 1431   |
| 1       | Senegal     | 2132   |
| 2       | New Zealand | 1402   |
| 4       | Croatia     | 1817   |
+---------+-------------+--------+
Bảng PointsChange:
+---------+---------------+
| team_id | points_change |
+---------+---------------+
| 3       | 399           |
| 2       | 0             |
| 4       | 13            |
| 1       | -22           |
+---------+---------------+
<strong>Đầu ra:</strong>
+---------+-------------+-----------+
| team_id | name        | rank_diff |
+---------+-------------+-----------+
| 1       | Senegal     | 0         |
| 4       | Croatia     | -1        |
| 3       | Algeria     | 1         |
| 2       | New Zealand | 0         |
+---------+-------------+-----------+
<strong>Giải thích:</strong>
Bảng xếp hạng toàn cầu ban đầu như sau:
+---------+-------------+--------+------+
| team_id | name        | points | rank |
+---------+-------------+--------+------+
| 1       | Senegal     | 2132   | 1    |
| 4       | Croatia     | 1817   | 2    |
| 3       | Algeria     | 1431   | 3    |
| 2       | New Zealand | 1402   | 4    |
+---------+-------------+--------+------+
Sau khi cập nhật điểm của mỗi đội, bảng xếp hạng trở thành:
+---------+-------------+--------+------+
| team_id | name        | points | rank |
+---------+-------------+--------+------+
| 1       | Senegal     | 2110   | 1    |
| 3       | Algeria     | 1830   | 2    |
| 4       | Croatia     | 1830   | 3    |
| 2       | New Zealand | 1402   | 4    |
+---------+-------------+--------+------+
Sau khi cập nhật điểm, Algeria và Croatia có cùng số điểm, vì vậy thứ hạng của họ được quyết định theo thứ tự từ điển.
Senegal mất 22 điểm nhưng thứ hạng không thay đổi.
Croatia tăng 13 điểm nhưng thứ hạng giảm một bậc.
Algeria tăng 399 điểm và thứ hạng tăng một bậc.
New Zealand không tăng hay giảm điểm, nên thứ hạng không thay đổi.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thứ hạng được xác định theo điểm giảm dần, sau đó theo tên tăng dần. Sau khi điểm thay đổi, chúng ta cần tính hiệu giữa hai thứ hạng. Các window function $\texttt{RANK}$ cung cấp cả hai thứ tự.
>
> Cộng các mức thay đổi theo từng đội, join với điểm ban đầu, rồi xếp hạng theo $\textit{points}$ và $\textit{points}+\textit{delta}$. Ép các thứ hạng về số nguyên có dấu trước khi trừ để hiệu không thể bị underflow.
>
> Chọn $\textit{team\_id}$, $\textit{name}$ và $\textit{rank\_diff}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT team_id, SUM(points_change) AS delta
        FROM PointsChange
        GROUP BY team_id
    )
SELECT
    team_id,
    name,
    CAST(RANK() OVER (ORDER BY points DESC, name) AS SIGNED) - CAST(
        RANK() OVER (ORDER BY (points + delta) DESC, name) AS SIGNED
    ) AS 'rank_diff'
FROM
    TeamPoints
    JOIN P USING (team_id);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
