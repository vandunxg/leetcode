---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3564. Seasonal Sales Analysis](https://leetcode.com/problems/seasonal-sales-analysis)

[中文文档](/solution/3500-3599/3564.Seasonal%20Sales%20Analysis/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>sales</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| sale_id       | int     |
| product_id    | int     |
| sale_date     | date    |
| quantity      | int     |
| price         | decimal |
+---------------+---------+
sale_id là mã định danh duy nhất của bảng này.
Mỗi hàng chứa thông tin về một lần bán sản phẩm, bao gồm product_id, ngày bán, số lượng bán ra và đơn giá.
</pre>

<p>Bảng: <code>products</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
| category      | varchar |
+---------------+---------+
product_id là mã định danh duy nhất của bảng này.
Mỗi hàng chứa thông tin về một sản phẩm, bao gồm tên và category của sản phẩm.
</pre>

<p>Hãy viết lời giải để tìm category phổ biến nhất trong mỗi mùa. Các mùa được định nghĩa như sau:</p>

<ul>
    <li><strong>Winter</strong>: tháng 12, tháng 1, tháng 2</li>
    <li><strong>Spring</strong>: tháng 3, tháng 4, tháng 5</li>
    <li><strong>Summer</strong>: tháng 6, tháng 7, tháng 8</li>
    <li><strong>Fall</strong>: tháng 9, tháng 10, tháng 11</li>
</ul>

<p><strong>Độ phổ biến</strong> của một <strong>category</strong> được xác định bởi <strong>tổng số lượng bán ra</strong> trong <strong>mùa đó</strong>. Nếu <strong>hòa</strong>, chọn category có <strong>tổng doanh thu</strong> cao hơn (<code>quantity &times; price</code>). Nếu vẫn hòa, trả về category có thứ tự từ điển nhỏ hơn.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo mùa theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng sales:</p>

<pre class="example-io">
+---------+------------+------------+----------+-------+
| sale_id | product_id | sale_date  | quantity | price |
+---------+------------+------------+----------+-------+
| 1       | 1          | 2023-01-15 | 5        | 10.00 |
| 2       | 2          | 2023-01-20 | 4        | 15.00 |
| 3       | 3          | 2023-03-10 | 3        | 18.00 |
| 4       | 4          | 2023-04-05 | 1        | 20.00 |
| 5       | 1          | 2023-05-20 | 2        | 10.00 |
| 6       | 2          | 2023-06-12 | 4        | 15.00 |
| 7       | 5          | 2023-06-15 | 5        | 12.00 |
| 8       | 3          | 2023-07-24 | 2        | 18.00 |
| 9       | 4          | 2023-08-01 | 5        | 20.00 |
| 10      | 5          | 2023-09-03 | 3        | 12.00 |
| 11      | 1          | 2023-09-25 | 6        | 10.00 |
| 12      | 2          | 2023-11-10 | 4        | 15.00 |
| 13      | 3          | 2023-12-05 | 6        | 18.00 |
| 14      | 4          | 2023-12-22 | 3        | 20.00 |
| 15      | 5          | 2024-02-14 | 2        | 12.00 |
+---------+------------+------------+----------+-------+
</pre>

<p>bảng products:</p>

<pre class="example-io">
+------------+-----------------+----------+
| product_id | product_name    | category |
+------------+-----------------+----------+
| 1          | Warm Jacket     | Apparel  |
| 2          | Designer Jeans  | Apparel  |
| 3          | Cutting Board   | Kitchen  |
| 4          | Smart Speaker   | Tech     |
| 5          | Yoga Mat        | Fitness  |
+------------+-----------------+----------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+----------+----------------+---------------+
| season  | category | total_quantity | total_revenue |
+---------+----------+----------------+---------------+
| Fall    | Apparel  | 10             | 120.00        |
| Spring  | Kitchen  | 3              | 54.00         |
| Summer  | Tech     | 5              | 100.00        |
| Winter  | Apparel  | 9              | 110.00        |
+---------+----------+----------------+---------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Fall (Sep, Oct, Nov):</strong>

    <ul>
        <li>Apparel: đã bán 10 sản phẩm (6 Jackets trong tháng 9, 4 Jeans trong tháng 11), doanh thu $120.00 (6&times;$10.00 + 4&times;$15.00)</li>
        <li>Fitness: đã bán 3 Yoga Mats trong tháng 9, doanh thu $36.00</li>
        <li>Phổ biến nhất: Apparel với tổng số lượng cao nhất (10)</li>
    </ul>
    </li>
    <li><strong>Spring (Mar, Apr, May):</strong>
    <ul>
        <li>Kitchen: đã bán 3 Cutting Boards trong tháng 3, doanh thu $54.00</li>
        <li>Tech: đã bán 1 Smart Speaker trong tháng 4, doanh thu $20.00</li>
        <li>Apparel: đã bán 2 Warm Jackets trong tháng 5, doanh thu $20.00</li>
        <li>Phổ biến nhất: Kitchen với tổng số lượng cao nhất (3) và doanh thu cao nhất ($54.00)</li>
    </ul>
    </li>
    <li><strong>Summer (Jun, Jul, Aug):</strong>
    <ul>
        <li>Apparel: đã bán 4 Designer Jeans trong tháng 6, doanh thu $60.00</li>
        <li>Fitness: đã bán 5 Yoga Mats trong tháng 6, doanh thu $60.00</li>
        <li>Kitchen: đã bán 2 Cutting Boards trong tháng 7, doanh thu $36.00</li>
        <li>Tech: đã bán 5 Smart Speakers trong tháng 8, doanh thu $100.00</li>
        <li>Phổ biến nhất: Tech và Fitness đều bán 5 sản phẩm, nhưng Tech có doanh thu cao hơn ($100.00 vs $60.00)</li>
    </ul>
    </li>
    <li><strong>Winter (Dec, Jan, Feb):</strong>
    <ul>
        <li>Apparel: đã bán 9 sản phẩm (5 Jackets trong tháng 1, 4 Jeans trong tháng 1), doanh thu $110.00</li>
        <li>Kitchen: đã bán 6 Cutting Boards trong tháng 12, doanh thu $108.00</li>
        <li>Tech: đã bán 3 Smart Speakers trong tháng 12, doanh thu $60.00</li>
        <li>Fitness: đã bán 2 Yoga Mats trong tháng 2, doanh thu $24.00</li>
        <li>Phổ biến nhất: Apparel với tổng số lượng cao nhất (9) và doanh thu cao nhất ($110.00)</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo mùa theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nối bằng đẳng thức + Gom nhóm và tính tổng + Hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Nối sales với products, ánh xạ các tháng vào bốn mùa, rồi tính tổng quantity và revenue theo $(\textit{season},\textit{category})$.
>
> Xếp hạng các category trong từng mùa theo quantity rồi đến revenue, giữ lại hạng $1$ và sắp xếp các hàng theo mùa. Việc xếp hạng bằng window giúp thay thế truy vấn con tương quan.

<!-- thinking:end -->

Ta có thể thực hiện equi join giữa bảng `sales` và bảng `products` để lấy category của từng bản ghi bán hàng. Tiếp theo, xác định mùa dựa trên tháng của ngày bán, sau đó nhóm theo mùa và category để tính tổng số lượng bán ra và tổng doanh thu. Cuối cùng, sử dụng hàm cửa sổ để xếp hạng các category trong từng mùa và chọn category có hạng cao nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    SeasonalSales AS (
        SELECT
            CASE
                WHEN MONTH(sale_date) IN (12, 1, 2) THEN 'Winter'
                WHEN MONTH(sale_date) IN (3, 4, 5) THEN 'Spring'
                WHEN MONTH(sale_date) IN (6, 7, 8) THEN 'Summer'
                WHEN MONTH(sale_date) IN (9, 10, 11) THEN 'Fall'
            END AS season,
            category,
            SUM(quantity) AS total_quantity,
            SUM(quantity * price) AS total_revenue
        FROM
            sales
            JOIN products USING (product_id)
        GROUP BY 1, 2
    ),
    TopCategoryPerSeason AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY season
                ORDER BY total_quantity DESC, total_revenue DESC
            ) AS rk
        FROM SeasonalSales
    )
SELECT season, category, total_quantity, total_revenue
FROM TopCategoryPerSeason
WHERE rk = 1
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def seasonal_sales_analysis(
    products: pd.DataFrame, sales: pd.DataFrame
) -> pd.DataFrame:
    df = sales.merge(products, on="product_id")
    month_to_season = {
        12: "Winter",
        1: "Winter",
        2: "Winter",
        3: "Spring",
        4: "Spring",
        5: "Spring",
        6: "Summer",
        7: "Summer",
        8: "Summer",
        9: "Fall",
        10: "Fall",
        11: "Fall",
    }
    df["season"] = df["sale_date"].dt.month.map(month_to_season)
    seasonal_sales = df.groupby(["season", "category"], as_index=False).agg(
        total_quantity=("quantity", "sum"),
        total_revenue=("quantity", lambda x: (x * df.loc[x.index, "price"]).sum()),
    )
    seasonal_sales["rk"] = (
        seasonal_sales.sort_values(
            ["season", "total_quantity", "total_revenue"],
            ascending=[True, False, False],
        )
        .groupby("season")
        .cumcount()
        + 1
    )
    result = seasonal_sales[seasonal_sales["rk"] == 1].copy()
    return result[
        ["season", "category", "total_quantity", "total_revenue"]
    ].sort_values("season")
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
