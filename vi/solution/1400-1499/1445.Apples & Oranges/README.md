---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1445. Apples & Oranges 🔒](https://leetcode.com/problems/apples-oranges)

[中文文档](/solution/1400-1499/1445.Apples%20%26%20Oranges/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Sales</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| sale_date     | date    |
| fruit         | enum    |
| sold_num      | int     |
+---------------+---------+
(sale_date, fruit) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng này chứa số lượng "apples" và "oranges" được bán mỗi ngày.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo chênh lệch giữa số lượng <strong>apples</strong> và <strong>oranges</strong> được bán mỗi ngày.</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>sale_date</code>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Sales table:
+------------+------------+-------------+
| sale_date  | fruit      | sold_num    |
+------------+------------+-------------+
| 2020-05-01 | apples     | 10          |
| 2020-05-01 | oranges    | 8           |
| 2020-05-02 | apples     | 15          |
| 2020-05-02 | oranges    | 15          |
| 2020-05-03 | apples     | 20          |
| 2020-05-03 | oranges    | 0           |
| 2020-05-04 | apples     | 15          |
| 2020-05-04 | oranges    | 16          |
+------------+------------+-------------+
<strong>Đầu ra:</strong>
+------------+--------------+
| sale_date  | diff         |
+------------+--------------+
| 2020-05-01 | 2            |
| 2020-05-02 | 0            |
| 2020-05-03 | 20           |
| 2020-05-04 | -1           |
+------------+--------------+
<strong>Giải thích:</strong>
Ngày 2020-05-01 bán 10 apples và 8 oranges (chênh lệch 10 - 8 = 2).
Ngày 2020-05-02 bán 15 apples và 15 oranges (chênh lệch 15 - 15 = 0).
Ngày 2020-05-03 bán 20 apples và 0 oranges (chênh lệch 20 - 0 = 20).
Ngày 2020-05-04 bán 15 apples và 16 oranges (chênh lệch 15 - 16 = -1).
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By + Sum

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày có một dòng cho apples và một dòng cho oranges. Nhóm theo `sale_date`, cộng số lượng apples và trừ số lượng oranges, sau đó sắp xếp theo ngày.

<!-- thinking:end -->

Ta có thể nhóm dữ liệu theo ngày, sau đó dùng hàm `sum` để tính chênh lệch số lượng apples và oranges được bán mỗi ngày. Nếu là apple, ta biểu diễn bằng một số dương; nếu là orange, ta biểu diễn bằng một số âm. Cuối cùng, sắp xếp dữ liệu theo ngày.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    sale_date,
    SUM(IF(fruit = 'apples', sold_num, -sold_num)) AS diff
FROM Sales
GROUP BY 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
