---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage)

[中文文档](/solution/1200-1299/1211.Queries%20Quality%20and%20Percentage/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Queries</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| query_name  | varchar |
| result      | varchar |
| position    | int     |
| rating      | int     |
+-------------+---------+
Bảng này có thể có các hàng trùng lặp.
Bảng này chứa thông tin thu thập được từ một số query trên database.
Cột <code>position</code> có giá trị từ <strong>1</strong> đến <strong>500</strong>.
Cột <code>rating</code> có giá trị từ <strong>1</strong> đến <strong>5</strong>. Query có <code>rating</code> nhỏ hơn 3 được xem là query kém.
</pre>

<p>&nbsp;</p>

<p>Định nghĩa <code>quality</code> của query là:</p>

<blockquote>
<p>Giá trị trung bình của tỉ số giữa rating và position của query.</p>
</blockquote>

<p>Định nghĩa <code>poor query percentage</code> là:</p>

<blockquote>
<p>Tỉ lệ phần trăm các query có rating nhỏ hơn 3 trên tổng số query.</p>
</blockquote>

<p>Hãy viết lời giải để tìm từng <code>query_name</code>, cùng <code>quality</code> và <code>poor_query_percentage</code>.</p>

<p>Cả <code>quality</code> và <code>poor_query_percentage</code> đều phải được <strong>làm tròn đến 2 chữ số thập phân</strong>.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Bảng Queries:
+------------+-------------------+----------+--------+
| query_name | result            | position | rating |
+------------+-------------------+----------+--------+
| Dog        | Golden Retriever  | 1        | 5      |
| Dog        | German Shepherd   | 2        | 5      |
| Dog        | Mule              | 200      | 1      |
| Cat        | Shirazi           | 5        | 2      |
| Cat        | Siamese           | 3        | 3      |
| Cat        | Sphynx            | 7        | 4      |
+------------+-------------------+----------+--------+
<strong>Output:</strong> 
+------------+---------+-----------------------+
| query_name | quality | poor_query_percentage |
+------------+---------+-----------------------+
| Dog        | 2.50    | 33.33                 |
| Cat        | 0.66    | 33.33                 |
+------------+---------+-----------------------+
<strong>Giải thích:</strong> 
Quality của các query Dog là ((5 / 1) + (5 / 2) + (1 / 200)) / 3 = 2.50
Poor_query_percentage của các query Dog là (1 / 3) * 100 = 33.33

Quality của các query Cat bằng ((2 / 5) + (3 / 3) + (4 / 7)) / 3 = 0.66
Poor_query_percentage của các query Cat là (1 / 3) * 100 = 33.33
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Quality là trung bình của $rating/position$; tỉ lệ query kém là phần query có $rating < 3$. Cả hai đều được tính riêng theo tên query, nên ta $GROUP\ BY\ query\_name$, lấy $AVG$ của tỉ số và biểu thức boolean, rồi $ROUND$ đến hai chữ số thập phân. Các query có tên null sẽ bị loại bỏ.

<!-- thinking:end -->

Nhóm theo `query_name`, tính `quality` bằng `AVG(rating / position)` và tính tỉ lệ query kém bằng `AVG(rating < 3)`. Làm tròn cả hai giá trị đến hai chữ số thập phân và loại các query có tên null bằng `WHERE query_name IS NOT NULL`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    query_name,
    ROUND(AVG(rating / position), 2) AS quality,
    ROUND(AVG(rating < 3) * 100, 2) AS poor_query_percentage
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Nhóm và tổng hợp (biểu thức CASE)

<!-- thinking:start -->

> **Tư duy**
>
> Không phải dialect nào cũng xử lý $AVG$ trên boolean rõ ràng như nhau. Lời giải 2 dùng $CAST$ để chia theo kiểu thập phân, đồng thời đếm các hàng query kém bằng $CASE$ kết hợp với $COUNT(*)$. Kết quả giống lời giải 1 và có tính tương thích cao hơn.

<!-- thinking:end -->

Cách nhóm vẫn như cũ và các query có tên null tiếp tục bị loại bỏ. `quality` dùng `CAST` để phép chia cho kết quả thập phân; tỉ lệ query kém được tính bằng biểu thức `CASE`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    query_name,
    ROUND(AVG(CAST(rating AS DECIMAL) / position), 2) AS quality,
    ROUND(
        SUM(
            CASE
                WHEN rating < 3 THEN 1
                ELSE 0
            END
        ) / COUNT(*) * 100,
        2
    ) AS poor_query_percentage
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY query_name;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
