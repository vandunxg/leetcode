---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3293. Calculate Product Final Price 🔒](https://leetcode.com/problems/calculate-product-final-price)

[中文文档](/solution/3200-3299/3293.Calculate%20Product%20Final%20Price/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace"><code>Products</code></font></p>

<pre>
+------------+---------+
| Column Name| Type    |
+------------+---------+
| product_id | int     |
| category   | varchar |
| price      | decimal |
+------------+---------+
product_id là khóa duy nhất của bảng này.
Mỗi hàng bao gồm ID sản phẩm, danh mục và giá của sản phẩm.
</pre>

<p>Bảng: <font face="monospace"><code>Discounts</code></font></p>

<pre>
+------------+---------+
| Column Name| Type    |
+------------+---------+
| category   | varchar |
| discount   | int     |
+------------+---------+
category là khóa chính của bảng này.
Mỗi hàng chứa một danh mục sản phẩm và phần trăm giảm giá áp dụng cho danh mục đó (giá trị nằm trong khoảng từ 0 đến 100).
</pre>

<p>Viết lời giải để tìm <strong>giá cuối cùng</strong> của từng sản phẩm sau khi áp dụng <strong>mức giảm giá theo danh mục</strong>. Nếu danh mục của sản phẩm <strong>không</strong> có <strong>mức giảm giá</strong> <strong>tương ứng</strong>, giá của sản phẩm vẫn <strong>không thay đổi</strong>.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>product_id</code><em> theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>Products</code>:</p>

<pre class="example-io">
+------------+-------------+-------+
| product_id | category    | price |
+------------+-------------+-------+
| 1          | Electronics | 1000  |
| 2          | Clothing    | 50    |
| 3          | Electronics | 1200  |
| 4          | Home        | 500   |
+------------+-------------+-------+
  </pre>

<p>Bảng <code>Discounts</code>:</p>

<pre class="example-io">
+------------+----------+
| category   | discount |
+------------+----------+
| Electronics| 10       |
| Clothing   | 20       |
+------------+----------+
  </pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------+------------+-------------+
| product_id | final_price| category    |
+------------+------------+-------------+
| 1          | 900        | Electronics |
| 2          | 40         | Clothing    |
| 3          | 1080       | Electronics |
| 4          | 500        | Home        |
+------------+------------+-------------+
  </pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sản phẩm 1 thuộc danh mục Electronics có mức giảm giá 10%, nên giá cuối cùng là 1000 - (10% của 1000) = 900.</li>
	<li>Sản phẩm 2 thuộc danh mục Clothing có mức giảm giá 20%, nên giá cuối cùng là 50 - (20% của 50) = 40.</li>
	<li>Sản phẩm 3 thuộc danh mục Electronics và được giảm giá 10%, nên giá cuối cùng là 1200 - (10% của 1200) = 1080.</li>
	<li>Không có mức giảm giá cho danh mục Home của sản phẩm 4, nên giá cuối cùng vẫn là 500.</li>
</ul>
Bảng kết quả được sắp xếp theo product_id theo thứ tự tăng dần.</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Giá cuối cùng bằng giá niêm yết nhân với $(100-\textit{discount})/100$, trong đó mức giảm giá bị thiếu được xem là $0$. Left join theo category giữ lại các sản phẩm không có mức giảm giá.
>
> Left-join `Discounts` vào `Products`, thay các mức giảm giá null bằng $0$, tính `final_price`, rồi sắp xếp theo `product_id`.

<!-- thinking:end -->

Ta có thể thực hiện left join giữa bảng `Products` và bảng `Discounts` trên cột `category`, sau đó tính giá cuối cùng. Nếu danh mục của sản phẩm không có mức giảm giá tương ứng, giá của sản phẩm không thay đổi.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    product_id,
    price * (100 - IFNULL(discount, 0)) / 100 final_price,
    category
FROM
    Products
    LEFT JOIN Discounts USING (category)
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def calculate_final_prices(
    products: pd.DataFrame, discounts: pd.DataFrame
) -> pd.DataFrame:
    # Perform a left join on the 'category' column
    merged_df = pd.merge(products, discounts, on="category", how="left")

    # Calculate the final price
    merged_df["final_price"] = (
        merged_df["price"] * (100 - merged_df["discount"].fillna(0)) / 100
    )

    # Select the necessary columns and sort by 'product_id'
    result_df = merged_df[["product_id", "final_price", "category"]].sort_values(
        "product_id"
    )

    return result_df
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
