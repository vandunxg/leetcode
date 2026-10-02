---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1699. Number of Calls Between Two Persons 🔒](https://leetcode.com/problems/number-of-calls-between-two-persons)

[中文文档](/solution/1600-1699/1699.Number%20of%20Calls%20Between%20Two%20Persons/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Calls</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| from_id     | int     |
| to_id       | int     |
| duration    | int     |
+-------------+---------+
Bảng này không có khóa chính (cột chứa các giá trị duy nhất), nên có thể chứa các dòng trùng lặp.
Bảng này chứa thời lượng của một cuộc gọi giữa from_id và to_id.
from_id != to_id
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo số lượng cuộc gọi và tổng thời lượng cuộc gọi giữa mỗi cặp người khác nhau <code>(person1, person2)</code> sao cho <code>person1 &lt; person2</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Calls table:
+---------+-------+----------+
| from_id | to_id | duration |
+---------+-------+----------+
| 1       | 2     | 59       |
| 2       | 1     | 11       |
| 1       | 3     | 20       |
| 3       | 4     | 100      |
| 3       | 4     | 200      |
| 3       | 4     | 200      |
| 4       | 3     | 499      |
+---------+-------+----------+
<strong>Đầu ra:</strong>
+---------+---------+------------+----------------+
| person1 | person2 | call_count | total_duration |
+---------+---------+------------+----------------+
| 1       | 2       | 2          | 70             |
| 1       | 3       | 1          | 20             |
| 3       | 4       | 4          | 999            |
+---------+---------+------------+----------------+
<strong>Giải thích:</strong>
Người dùng 1 và 2 có 2 cuộc gọi với tổng thời lượng là 70 (59 + 11).
Người dùng 1 và 3 có 1 cuộc gọi với tổng thời lượng là 20.
Người dùng 3 và 4 có 4 cuộc gọi với tổng thời lượng là 999 (100 + 200 + 200 + 499).
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Các cuộc gọi là vô hướng, nên $(a,b)$ và $(b,a)$ là cùng một cặp. Chuẩn hóa ID nhỏ hơn thành $\texttt{person1}$ và ID lớn hơn thành $\texttt{person2}$, rồi nhóm lại.
>
> $\texttt{IF}$ chuẩn hóa hai đầu mút; $\texttt{GROUP BY}$ hai cột đó sẽ cho số lượng và tổng thời lượng.

<!-- thinking:end -->

Ta có thể sử dụng hàm `if` hoặc các hàm `least` và `greatest` để chuyển `from_id` và `to_id` thành `person1` và `person2`, sau đó group by `person1` và `person2` rồi tính tổng các giá trị.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    IF(from_id < to_id, from_id, to_id) AS person1,
    IF(from_id < to_id, to_id, from_id) AS person2,
    COUNT(1) AS call_count,
    SUM(duration) AS total_duration
FROM Calls
GROUP BY 1, 2;
```

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    LEAST(from_id, to_id) AS person1,
    GREATEST(from_id, to_id) AS person2,
    COUNT(1) AS call_count,
    SUM(duration) AS total_duration
FROM Calls
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
