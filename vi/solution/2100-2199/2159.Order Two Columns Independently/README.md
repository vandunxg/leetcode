---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2159. Order Two Columns Independently 🔒](https://leetcode.com/problems/order-two-columns-independently)

[中文文档](/solution/2100-2199/2159.Order%20Two%20Columns%20Independently/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Data</code></p>

<pre>
+-------------+------+
| Tên cột     | Kiểu |
+-------------+------+
| first_col   | int  |
| second_col  | int  |
+-------------+------+
Bảng này có thể chứa các hàng trùng lặp.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để thực hiện độc lập các yêu cầu sau:</p>

<ul>
	<li>sắp xếp <code>first_col</code> theo <strong>thứ tự tăng dần</strong>.</li>
	<li>sắp xếp <code>second_col</code> theo <strong>thứ tự giảm dần</strong>.</li>
</ul>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Data:
+-----------+------------+
| first_col | second_col |
+-----------+------------+
| 4         | 2          |
| 2         | 3          |
| 3         | 1          |
| 1         | 4          |
+-----------+------------+
<strong>Đầu ra:</strong>
+-----------+------------+
| first_col | second_col |
+-----------+------------+
| 1         | 4          |
| 2         | 3          |
| 3         | 2          |
| 4         | 1          |
+-----------+------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai cột phải được sắp xếp độc lập: cột thứ nhất theo thứ tự tăng dần, cột thứ hai theo thứ tự giảm dần, sau đó ghép tương ứng theo từng hàng. Nếu sắp xếp bảng theo từng cặp, mối liên kết ban đầu giữa các giá trị sẽ được giữ nguyên.
>
> Các window function gán thứ hạng cho từng cột; join theo thứ hạng sẽ ghép các giá trị đã được sắp xếp độc lập vào đúng hàng.
>
> Đánh số $\textit{first\_col}$ theo thứ tự tăng dần và $\textit{second\_col}$ theo thứ tự giảm dần, sau đó join theo $\textit{rk}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT
            first_col,
            ROW_NUMBER() OVER (ORDER BY first_col) AS rk
        FROM Data
    ),
    T AS (
        SELECT
            second_col,
            ROW_NUMBER() OVER (ORDER BY second_col DESC) AS rk
        FROM Data
    )
SELECT first_col, second_col
FROM
    S
    JOIN T USING (rk);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
