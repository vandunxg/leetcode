---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3626. Find Stores with Inventory Imbalance](https://leetcode.com/problems/find-stores-with-inventory-imbalance)

[中文文档](/solution/3600-3699/3626.Find%20Stores%20with%20Inventory%20Imbalance/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>stores</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| store_id    | int     |
| store_name  | varchar |
| location    | varchar |
+-------------+---------+
store_id là định danh duy nhất của bảng này.
Mỗi dòng chứa thông tin về một cửa hàng và vị trí của cửa hàng đó.
</pre>

<p>Bảng: <code>inventory</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| inventory_id| int     |
| store_id    | int     |
| product_name| varchar |
| quantity    | int     |
| price       | decimal |
+-------------+---------+
inventory_id là định danh duy nhất của bảng này.
Mỗi dòng biểu diễn tồn kho của một sản phẩm cụ thể tại một cửa hàng cụ thể.
</pre>

<p>Hãy viết lời giải để tìm các cửa hàng có <strong>mất cân bằng tồn kho</strong> - tức là các cửa hàng mà sản phẩm đắt nhất có lượng tồn kho thấp hơn sản phẩm rẻ nhất.</p>

<ul>
    <li>Với mỗi cửa hàng, xác định <strong>sản phẩm đắt nhất</strong> (giá cao nhất) và số lượng của sản phẩm đó</li>
    <li>Với mỗi cửa hàng, xác định <strong>sản phẩm rẻ nhất</strong> (giá thấp nhất) và số lượng của sản phẩm đó</li>
    <li>Một cửa hàng có mất cân bằng tồn kho nếu số lượng của sản phẩm đắt nhất <strong>nhỏ hơn</strong> số lượng của sản phẩm rẻ nhất</li>
    <li>Tính <strong>tỷ lệ mất cân bằng</strong> theo công thức (cheapest_quantity / most_expensive_quantity)</li>
    <li><strong>Làm tròn</strong> tỷ lệ mất cân bằng đến <strong>2</strong> chữ số thập phân</li>
    <li>Chỉ đưa vào kết quả các cửa hàng có <strong>ít nhất </strong><code>3</code><strong> sản phẩm khác nhau</strong></li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo tỷ lệ mất cân bằng <strong>giảm dần</strong>, sau đó theo tên cửa hàng <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng stores:</p>

<pre class="example-io">
+----------+----------------+-------------+
| store_id | store_name     | location    |
+----------+----------------+-------------+
| 1        | Downtown Tech  | New York    |
| 2        | Suburb Mall    | Chicago     |
| 3        | City Center    | Los Angeles |
| 4        | Corner Shop    | Miami       |
| 5        | Plaza Store    | Seattle     |
+----------+----------------+-------------+
</pre>

<p>bảng inventory:</p>

<pre class="example-io">
+--------------+----------+--------------+----------+--------+
| inventory_id | store_id | product_name | quantity | price  |
+--------------+----------+--------------+----------+--------+
| 1            | 1        | Laptop       | 5        | 999.99 |
| 2            | 1        | Mouse        | 50       | 19.99  |
| 3            | 1        | Keyboard     | 25       | 79.99  |
| 4            | 1        | Monitor      | 15       | 299.99 |
| 5            | 2        | Phone        | 3        | 699.99 |
| 6            | 2        | Charger      | 100      | 25.99  |
| 7            | 2        | Case         | 75       | 15.99  |
| 8            | 2        | Headphones   | 20       | 149.99 |
| 9            | 3        | Tablet       | 2        | 499.99 |
| 10           | 3        | Stylus       | 80       | 29.99  |
| 11           | 3        | Cover        | 60       | 39.99  |
| 12           | 4        | Watch        | 10       | 299.99 |
| 13           | 4        | Band         | 25       | 49.99  |
| 14           | 5        | Camera       | 8        | 599.99 |
| 15           | 5        | Lens         | 12       | 199.99 |
+--------------+----------+--------------+----------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+----------+----------------+-------------+------------------+--------------------+------------------+
| store_id | store_name     | location    | most_exp_product | cheapest_product   | imbalance_ratio  |
+----------+----------------+-------------+------------------+--------------------+------------------+
| 3        | City Center    | Los Angeles | Tablet           | Stylus             | 40.00            |
| 2        | Suburb Mall    | Chicago     | Phone            | Case               | 25.00            |
| 1        | Downtown Tech  | New York    | Laptop           | Mouse              | 10.00            |
+----------+----------------+-------------+------------------+--------------------+------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Downtown Tech (store_id = 1):</strong>

    <ul>
         <li>Sản phẩm đắt nhất: Laptop ($999.99), có số lượng 5</li>
         <li>Sản phẩm rẻ nhất: Mouse ($19.99), có số lượng 50</li>
         <li>Mất cân bằng tồn kho: 5 &lt; 50 (sản phẩm đắt hơn có lượng tồn kho thấp hơn)</li>
         <li>Tỷ lệ mất cân bằng: 50 / 5 = 10.00</li>
         <li>Có 4 sản phẩm (&ge; 3), nên đủ điều kiện</li>
    </ul>
    </li>
    <li><strong>Suburb Mall (store_id = 2):</strong>
    <ul>
         <li>Sản phẩm đắt nhất: Phone ($699.99), có số lượng 3</li>
         <li>Sản phẩm rẻ nhất: Case ($15.99), có số lượng 75</li>
         <li>Mất cân bằng tồn kho: 3 &lt; 75 (sản phẩm đắt hơn có lượng tồn kho thấp hơn)</li>
         <li>Tỷ lệ mất cân bằng: 75 / 3 = 25.00</li>
         <li>Có 4 sản phẩm (&ge; 3), nên đủ điều kiện</li>
    </ul>
    </li>
    <li><strong>City Center (store_id = 3):</strong>
    <ul>
         <li>Sản phẩm đắt nhất: Tablet ($499.99), có số lượng 2</li>
         <li>Sản phẩm rẻ nhất: Stylus ($29.99), có số lượng 80</li>
         <li>Mất cân bằng tồn kho: 2 &lt; 80 (sản phẩm đắt hơn có lượng tồn kho thấp hơn)</li>
         <li>Tỷ lệ mất cân bằng: 80 / 2 = 40.00</li>
         <li>Có 3 sản phẩm (&ge; 3), nên đủ điều kiện</li>
    </ul>
    </li>
    <li><strong>Các cửa hàng không được đưa vào:</strong>
    <ul>
         <li>Corner Shop (store_id = 4): Chỉ có 2 sản phẩm (Watch, Band), không đáp ứng yêu cầu tối thiểu 3 sản phẩm</li>
         <li>Plaza Store (store_id = 5): Chỉ có 2 sản phẩm (Camera, Lens), không đáp ứng yêu cầu tối thiểu 3 sản phẩm</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo tỷ lệ mất cân bằng giảm dần, sau đó theo tên cửa hàng tăng dần</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Window Functions + Joins

<!-- thinking:start -->

> **Tư duy**
>
> Mất cân bằng nghĩa là một cửa hàng có ít nhất ba sản phẩm và lượng tồn kho của sản phẩm đắt nhất thấp hơn nghiêm ngặt so với sản phẩm rẻ nhất. Nếu tự duyệt từng cửa hàng, chúng ta dễ xử lý sai khi các sản phẩm có cùng giá.
>
> Chỉ giữ lại các cửa hàng có đủ số lượng sản phẩm khác nhau. Sắp xếp inventory theo $(\textit{store\_id},\textit{price},\textit{quantity})$ và lấy dòng đầu tiên của mỗi cửa hàng theo từng hướng; sắp xếp quantity giảm dần giúp phá vỡ trường hợp các sản phẩm có cùng giá một cách duy nhất.
>
> Join hai bảng kết quả, giữ lại các dòng mà số lượng của sản phẩm đắt nhất nhỏ hơn sản phẩm rẻ nhất, làm tròn tỷ lệ đến hai chữ số thập phân, join với thông tin cửa hàng, rồi sắp xếp theo tỷ lệ và tên.

<!-- thinking:end -->

Chúng ta có thể sử dụng window function để tính sản phẩm đắt nhất và rẻ nhất của mỗi cửa hàng, rồi dùng join để lọc các cửa hàng bị mất cân bằng tồn kho. Các bước cụ thể như sau:

1. **Tính sản phẩm đắt nhất của mỗi cửa hàng**: Dùng window function `RANK()` để sắp xếp theo giá giảm dần; nếu giá bằng nhau thì sắp xếp theo số lượng giảm dần, và chọn sản phẩm có hạng đầu tiên.
2. **Tính sản phẩm rẻ nhất của mỗi cửa hàng**: Dùng window function `RANK()` để sắp xếp theo giá tăng dần; nếu giá bằng nhau thì sắp xếp theo số lượng giảm dần, và chọn sản phẩm có hạng đầu tiên.
3. **Lọc các cửa hàng có ít nhất 3 sản phẩm khác nhau**: Dùng window function `COUNT()` để đếm số sản phẩm của mỗi cửa hàng, rồi chỉ giữ các cửa hàng có số lượng lớn hơn hoặc bằng 3.
4. **Join sản phẩm đắt nhất và rẻ nhất**: Join kết quả của sản phẩm đắt nhất và rẻ nhất, đồng thời đảm bảo số lượng của sản phẩm đắt nhất nhỏ hơn số lượng của sản phẩm rẻ nhất.
5. **Tính tỷ lệ mất cân bằng**: Tính tỷ lệ giữa số lượng của sản phẩm rẻ nhất và số lượng của sản phẩm đắt nhất, rồi làm tròn đến hai chữ số thập phân.
6. **Join thông tin cửa hàng**: Join kết quả với bảng thông tin cửa hàng để lấy tên và vị trí cửa hàng.
7. **Sắp xếp kết quả**: Sắp xếp theo tỷ lệ mất cân bằng giảm dần, sau đó theo tên cửa hàng tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            store_id,
            product_name,
            quantity,
            RANK() OVER (
                PARTITION BY store_id
                ORDER BY price DESC, quantity DESC
            ) rk1,
            RANK() OVER (
                PARTITION BY store_id
                ORDER BY price, quantity DESC
            ) rk2,
            COUNT(1) OVER (PARTITION BY store_id) cnt
        FROM inventory
    ),
    P1 AS (
        SELECT *
        FROM T
        WHERE rk1 = 1 AND cnt >= 3
    ),
    P2 AS (
        SELECT *
        FROM T
        WHERE rk2 = 1
    )
SELECT
    s.store_id store_id,
    store_name,
    location,
    p1.product_name most_exp_product,
    p2.product_name cheapest_product,
    ROUND(p2.quantity / p1.quantity, 2) imbalance_ratio
FROM
    P1 p1
    JOIN P2 p2 ON p1.store_id = p2.store_id AND p1.quantity < p2.quantity
    JOIN stores s ON p1.store_id = s.store_id
ORDER BY imbalance_ratio DESC, store_name;
```

#### Pandas

```python
import pandas as pd


def find_inventory_imbalance(
    stores: pd.DataFrame, inventory: pd.DataFrame
) -> pd.DataFrame:
    # Step 1: Identify stores with at least 3 products
    store_counts = inventory["store_id"].value_counts()
    valid_stores = store_counts[store_counts >= 3].index

    # Step 2: Find most expensive product for each valid store
    # Sort by price (descending) then quantity (descending) and take first record per store
    most_expensive = (
        inventory[inventory["store_id"].isin(valid_stores)]
        .sort_values(["store_id", "price", "quantity"], ascending=[True, False, False])
        .groupby("store_id")
        .first()
        .reset_index()
    )

    # Step 3: Find cheapest product for each store
    # Sort by price (ascending) then quantity (descending) and take first record per store
    cheapest = (
        inventory.sort_values(
            ["store_id", "price", "quantity"], ascending=[True, True, False]
        )
        .groupby("store_id")
        .first()
        .reset_index()
    )

    # Step 4: Merge the two datasets on store_id
    merged = pd.merge(
        most_expensive, cheapest, on="store_id", suffixes=("_most", "_cheap")
    )

    # Step 5: Filter for cases where cheapest product has higher quantity than most expensive
    result = merged[merged["quantity_most"] < merged["quantity_cheap"]].copy()

    # Step 6: Calculate imbalance ratio (cheapest quantity / most expensive quantity)
    result["imbalance_ratio"] = (
        result["quantity_cheap"] / result["quantity_most"]
    ).round(2)

    # Step 7: Merge with store information to get store names and locations
    result = pd.merge(result, stores, on="store_id")

    # Step 8: Select and rename columns for final output
    result = result[
        [
            "store_id",
            "store_name",
            "location",
            "product_name_most",
            "product_name_cheap",
            "imbalance_ratio",
        ]
    ].rename(
        columns={
            "product_name_most": "most_exp_product",
            "product_name_cheap": "cheapest_product",
        }
    )

    # Step 9: Sort by imbalance ratio (descending) then store name (ascending)
    result = result.sort_values(
        ["imbalance_ratio", "store_name"], ascending=[False, True]
    ).reset_index(drop=True)

    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
