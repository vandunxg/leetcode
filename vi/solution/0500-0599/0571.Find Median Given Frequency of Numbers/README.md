---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [571. Find Median Given Frequency of Numbers 🔒](https://leetcode.com/problems/find-median-given-frequency-of-numbers)

[中文文档](/solution/0500-0599/0571.Find%20Median%20Given%20Frequency%20of%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Numbers</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| num         | int  |
| frequency   | int  |
+-------------+------+
num là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng của bảng cho biết tần suất xuất hiện của một số trong database.
</pre>

<p>&nbsp;</p>

<p><a href="https://en.wikipedia.org/wiki/Median" target="_blank"><strong>Median</strong></a> là giá trị chia mẫu dữ liệu thành hai nửa: nửa thấp hơn và nửa cao hơn.</p>

<p>Hãy viết lời giải để tìm <strong>median</strong> của tất cả các số trong database sau khi giải nén bảng <code>Numbers</code>. Làm tròn median đến <strong>một chữ số thập phân</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Numbers:
+-----+-----------+
| num | frequency |
+-----+-----------+
| 0   | 7         |
| 1   | 1         |
| 2   | 3         |
| 3   | 1         |
+-----+-----------+
<strong>Đầu ra:</strong> 
+--------+
| median |
+--------+
| 0.0    |
+--------+
<strong>Giải thích:</strong> 
Khi giải nén bảng Numbers, ta thu được [0, 0, 0, 0, 0, 0, 0, 1, 2, 2, 2, 3], nên median là (0 + 0) / 2 = 0.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giá trị có một tần suất; median nằm ở giữa dãy sau khi giải nén. Không cần tạo dãy đã giải nén ra.
>
> Tính tần suất prefix từ trái (`rk1`) và từ phải (`rk2`), trong đó $s$ là tổng số phần tử. Các hàng thỏa $rk1 \ge s/2$ và $rk2 \ge s/2$ chứa giá trị ở giữa hoặc hai giá trị giữa; lấy trung bình các giá trị đó. `ROUND(..., 1)` giữ lại một chữ số thập phân.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            *,
            SUM(frequency) OVER (ORDER BY num ASC) AS rk1,
            SUM(frequency) OVER (ORDER BY num DESC) AS rk2,
            SUM(frequency) OVER () AS s
        FROM Numbers
    )
SELECT
    ROUND(AVG(num), 1) AS median
FROM t
WHERE rk1 >= s / 2 AND rk2 >= s / 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
