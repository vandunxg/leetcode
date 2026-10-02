---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1384. Total Sales Amount by Year 🔒](https://leetcode.com/problems/total-sales-amount-by-year)

[中文文档](/solution/1300-1399/1384.Total%20Sales%20Amount%20by%20Year/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Product</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
+---------------+---------+
product_id là khóa chính (cột có giá trị duy nhất) của bảng này.
product_name là tên sản phẩm.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Sales</code></p>

<pre>
+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| product_id          | int     |
| period_start        | date    |
| period_end          | date    |
| average_daily_sales | int     |
+---------------------+---------+
product_id là khóa chính (cột có giá trị duy nhất) của bảng này. 
period_start và period_end lần lượt là ngày bắt đầu và kết thúc kỳ bán hàng; cả hai ngày đều được tính.
Cột average_daily_sales lưu doanh thu trung bình mỗi ngày của sản phẩm trong kỳ.
Các năm có dữ liệu bán hàng nằm trong khoảng từ 2018 đến 2020.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo tổng doanh thu hằng năm của từng sản phẩm, kèm theo <code>product_name</code>, <code>product_id</code>, <code>report_year</code> và <code>total_amount</code>.</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>product_id</code> và <code>report_year</code>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Product:
+------------+--------------+
| product_id | product_name |
+------------+--------------+
| 1          | LC Phone     |
| 2          | LC T-Shirt   |
| 3          | LC Keychain  |
+------------+--------------+
Bảng Sales:
+------------+--------------+-------------+---------------------+
| product_id | period_start | period_end  | average_daily_sales |
+------------+--------------+-------------+---------------------+
| 1          | 2019-01-25   | 2019-02-28  | 100                 |
| 2          | 2018-12-01   | 2020-01-01  | 10                  |
| 3          | 2019-12-01   | 2020-01-31  | 1                   |
+------------+--------------+-------------+---------------------+
<strong>Đầu ra:</strong> 
+------------+--------------+-------------+--------------+
| product_id | product_name | report_year | total_amount |
+------------+--------------+-------------+--------------+
| 1          | LC Phone     |    2019     | 3500         |
| 2          | LC T-Shirt   |    2018     | 310          |
| 2          | LC T-Shirt   |    2019     | 3650         |
| 2          | LC T-Shirt   |    2020     | 10           |
| 3          | LC Keychain  |    2019     | 31           |
| 3          | LC Keychain  |    2020     | 31           |
+------------+--------------+-------------+--------------+
<strong>Giải thích:</strong> 
LC Phone được bán trong khoảng từ 2019-01-25 đến 2019-02-28, gồm 35 ngày. Tổng doanh thu là 35*100 = 3500. 
LC T-shirt được bán trong khoảng từ 2018-12-01 đến 2020-01-01; số ngày bán tương ứng trong các năm 2018, 2019 và 2020 là 31, 365 và 1.
LC Keychain được bán trong khoảng từ 2019-12-01 đến 2020-01-31; số ngày bán trong các năm 2019 và 2020 lần lượt là 31 và 31.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tổng doanh thu có thể trải qua các năm $2018$– $2020$. Ghép mỗi đợt bán hàng với những năm dương lịch mà đợt đó giao nhau; số ngày trong năm được tính bằng ngày thứ bao nhiêu trong năm của ngày kết thúc trừ ngày thứ bao nhiêu trong năm của ngày bắt đầu, cộng một, rồi nhân với doanh thu trung bình mỗi ngày. Năm $2020$ có $366$ ngày.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    s.product_id,
    p.product_name,
    y.YEAR report_year,
    s.average_daily_sales * (
        IF(YEAR(s.period_end) > y.YEAR, y.days_of_year, DAYOFYEAR(s.period_end)) - IF(
            YEAR(s.period_start) < y.YEAR,
            1,
            DAYOFYEAR(s.period_start)
        ) + 1
    ) total_amount
FROM
    Sales s
    INNER JOIN (
        SELECT
            '2018' YEAR,
            365 days_of_year
        UNION ALL
        SELECT
            '2019' YEAR,
            365 days_of_year
        UNION ALL
        SELECT
            '2020' YEAR,
            366 days_of_year
    ) y
        ON YEAR(s.period_start) <= y.YEAR AND YEAR(s.period_end) >= y.YEAR
    INNER JOIN Product p ON p.product_id = s.product_id
ORDER BY s.product_id, y.YEAR;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
