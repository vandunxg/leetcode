---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [601. Human Traffic of Stadium](https://leetcode.com/problems/human-traffic-of-stadium)

[中文文档](/solution/0600-0699/0601.Human%20Traffic%20of%20Stadium/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Stadium</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| visit_date    | date    |
| people        | int     |
+---------------+---------+
visit_date là cột có các giá trị duy nhất trong bảng này.
Mỗi hàng trong bảng ghi ngày và id của lượt ghé thăm sân vận động, cùng số người có mặt trong lượt đó.
Khi id tăng thì ngày cũng tăng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để hiển thị các bản ghi thuộc những nhóm có từ ba hàng trở lên với <code>id</code> <strong>liên tiếp</strong>, trong đó mỗi hàng có số người lớn hơn hoặc bằng 100.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>visit_date</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Stadium:
+------+------------+-----------+
| id   | visit_date | people    |
+------+------------+-----------+
| 1    | 2017-01-01 | 10        |
| 2    | 2017-01-02 | 109       |
| 3    | 2017-01-03 | 150       |
| 4    | 2017-01-04 | 99        |
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-09 | 188       |
+------+------------+-----------+
<strong>Đầu ra:</strong> 
+------+------------+-----------+
| id   | visit_date | people    |
+------+------------+-----------+
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-09 | 188       |
+------+------------+-----------+
<strong>Giải thích:</strong> 
Bốn hàng có id 5, 6, 7 và 8 có id liên tiếp, đồng thời mỗi hàng đều có ít nhất 100 người tham dự. Lưu ý hàng 8 vẫn được đưa vào dù visit_date không phải ngày kế tiếp sau hàng 7.
Các hàng có id 2 và 3 không được đưa vào vì cần ít nhất ba id liên tiếp.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần ít nhất ba hàng có id liên tiếp và `people >= 100`. Self-join ba bảng có thể xử lý các nhóm dài đúng $3$, nhưng sẽ khó xử lý các nhóm dài hơn.
>
> Sau khi lọc các hàng thỏa điều kiện, những `id` liên tiếp sẽ có cùng giá trị `id - ROW_NUMBER()`. Nhóm theo hiệu này, đếm số hàng trong mỗi nhóm và giữ các nhóm có ít nhất $3$ hàng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT
            *,
            id - (ROW_NUMBER() OVER (ORDER BY id)) AS rk
        FROM Stadium
        WHERE people >= 100
    ),
    T AS (SELECT *, COUNT(1) OVER (PARTITION BY rk) AS cnt FROM S)
SELECT id, visit_date, people
FROM T
WHERE cnt >= 3
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã dùng window function để đếm kích thước nhóm. Có thể gom nhóm theo cùng hiệu `id_diff`, dùng `HAVING COUNT(*) > 2`, rồi lọc bằng `IN`. Ý tưởng không đổi, chỉ khác cách đếm.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    Consecutive AS (
        SELECT
            *,
            id - ROW_NUMBER() OVER () AS id_diff
        FROM Stadium
        WHERE people >= 100
    )
SELECT id, visit_date, people
FROM Consecutive
WHERE
    id_diff IN (
        SELECT id_diff
        FROM Consecutive
        GROUP BY id_diff
        HAVING COUNT(*) > 2
    )
ORDER BY visit_date;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
