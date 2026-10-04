---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3554. Find Category Recommendation Pairs](https://leetcode.com/problems/find-category-recommendation-pairs)

[中文文档](/solution/3500-3599/3554.Find%20Category%20Recommendation%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>ProductPurchases</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| product_id  | int  |
| quantity    | int  |
+-------------+------+
(user_id, product_id) là khóa duy nhất của bảng này.
Mỗi hàng thể hiện việc một người dùng mua một sản phẩm với một số lượng cụ thể.
</pre>

<p>Bảng: <code>ProductInfo</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| category    | varchar |
| price       | decimal |
+-------------+---------+
product_id là khóa duy nhất của bảng này.
Mỗi hàng gán một danh mục và giá cho một sản phẩm.
</pre>

<p>Amazon muốn hiểu các mô hình mua sắm giữa các danh mục sản phẩm. Hãy viết lời giải để:</p>

<ol>
    <li>Tìm tất cả <strong>cặp danh mục</strong> (trong đó <code>category1</code> &lt; <code>category2</code>)</li>
    <li>Với <strong>mỗi cặp danh mục</strong>, xác định số lượng <strong>khách hàng</strong> <strong>khác nhau</strong> đã mua sản phẩm thuộc <strong>cả hai</strong> danh mục</li>
</ol>

<p>Một cặp danh mục <strong>được đưa vào kết quả</strong> nếu có ít nhất <code>3</code> khách hàng khác nhau đã mua sản phẩm thuộc cả hai danh mục.</p>

<p><em>Trả về bảng kết quả gồm các cặp danh mục được đưa vào kết quả, sắp xếp theo <strong>customer_count</strong> theo <strong>thứ tự giảm dần</strong>; nếu bằng nhau, sắp xếp theo <strong>category1</strong> theo <strong>thứ tự tăng dần</strong> về mặt từ điển, sau đó theo <strong>category2</strong> theo <strong>thứ tự tăng dần</strong>.</em></p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng ProductPurchases:</p>

<pre class="example-io">
+---------+------------+----------+
| user_id | product_id | quantity |
+---------+------------+----------+
| 1       | 101        | 2        |
| 1       | 102        | 1        |
| 1       | 201        | 3        |
| 1       | 301        | 1        |
| 2       | 101        | 1        |
| 2       | 102        | 2        |
| 2       | 103        | 1        |
| 2       | 201        | 5        |
| 3       | 101        | 2        |
| 3       | 103        | 1        |
| 3       | 301        | 4        |
| 3       | 401        | 2        |
| 4       | 101        | 1        |
| 4       | 201        | 3        |
| 4       | 301        | 1        |
| 4       | 401        | 2        |
| 5       | 102        | 2        |
| 5       | 103        | 1        |
| 5       | 201        | 2        |
| 5       | 202        | 3        |
+---------+------------+----------+
</pre>

<p>Bảng ProductInfo:</p>

<pre class="example-io">
+------------+-------------+-------+
| product_id | category    | price |
+------------+-------------+-------+
| 101        | Electronics | 100   |
| 102        | Books       | 20    |
| 103        | Books       | 35    |
| 201        | Clothing    | 45    |
| 202        | Clothing    | 60    |
| 301        | Sports      | 75    |
| 401        | Kitchen     | 50    |
+------------+-------------+-------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+-------------+----------------+
| category1   | category2   | customer_count |
+-------------+-------------+----------------+
| Books       | Clothing    | 3              |
| Books       | Electronics | 3              |
| Clothing    | Electronics | 3              |
| Electronics | Sports      | 3              |
+-------------+-------------+----------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Books-Clothing</strong>:

    <ul>
        <li>Người dùng 1 đã mua sản phẩm thuộc Books (102) và Clothing (201)</li>
        <li>Người dùng 2 đã mua sản phẩm thuộc Books (102, 103) và Clothing (201)</li>
        <li>Người dùng 5 đã mua sản phẩm thuộc Books (102, 103) và Clothing (201, 202)</li>
        <li>Tổng cộng: 3 khách hàng đã mua sản phẩm thuộc cả hai danh mục</li>
    </ul>
    </li>
    <li><strong>Books-Electronics</strong>:
    <ul>
        <li>Người dùng 1 đã mua sản phẩm thuộc Books (102) và Electronics (101)</li>
        <li>Người dùng 2 đã mua sản phẩm thuộc Books (102, 103) và Electronics (101)</li>
        <li>Người dùng 3 đã mua sản phẩm thuộc Books (103) và Electronics (101)</li>
        <li>Tổng cộng: 3 khách hàng đã mua sản phẩm thuộc cả hai danh mục</li>
    </ul>
    </li>
    <li><strong>Clothing-Electronics</strong>:
    <ul>
        <li>Người dùng 1 đã mua sản phẩm thuộc Clothing (201) và Electronics (101)</li>
        <li>Người dùng 2 đã mua sản phẩm thuộc Clothing (201) và Electronics (101)</li>
        <li>Người dùng 4 đã mua sản phẩm thuộc Clothing (201) và Electronics (101)</li>
        <li>Tổng cộng: 3 khách hàng đã mua sản phẩm thuộc cả hai danh mục</li>
    </ul>
    </li>
    <li><strong>Electronics-Sports</strong>:
    <ul>
        <li>Người dùng 1 đã mua sản phẩm thuộc Electronics (101) và Sports (301)</li>
        <li>Người dùng 3 đã mua sản phẩm thuộc Electronics (101) và Sports (301)</li>
        <li>Người dùng 4 đã mua sản phẩm thuộc Electronics (101) và Sports (301)</li>
        <li>Tổng cộng: 3 khách hàng đã mua sản phẩm thuộc cả hai danh mục</li>
    </ul>
    </li>
    <li>Các cặp danh mục khác như Clothing-Sports (chỉ có 2 khách hàng: người dùng 1 và 4) và Books-Kitchen (chỉ có 1 khách hàng: người dùng 3) có ít hơn 3 khách hàng chung nên không được đưa vào kết quả.</li>

</ul>

<p>Kết quả được sắp xếp theo customer_count theo thứ tự giảm dần. Vì tất cả các cặp đều có customer_count bằng 3, chúng được sắp xếp theo category1 (sau đó là category2) theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Join + Gom nhóm và tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp đề xuất là hai danh mục khác nhau được ít nhất ba người dùng mua cùng nhau. Join bảng giao dịch với thông tin sản phẩm, sau đó loại bỏ các hàng $(\textit{user\_id},\textit{category})$ trùng lặp.
>
> Self-join các danh mục của cùng một người dùng để tạo các cặp có thứ tự, đếm số người dùng khác nhau, giữ lại các cặp có số lượng $\ge 3$, rồi sắp xếp theo số lượng và tên danh mục.

<!-- thinking:end -->

Trước tiên, ta join bảng `ProductPurchases` với bảng `ProductInfo` theo `product_id` để tạo bảng `user_category` gồm `user_id` và `category`. Tiếp theo, ta self-join bảng `user_category` để lấy tất cả các cặp danh mục mà mỗi người dùng đã mua. Cuối cùng, ta gom nhóm các cặp danh mục này, đếm số người dùng cho mỗi cặp và lọc ra các cặp có ít nhất 3 người dùng.

Sau cùng, ta sắp xếp kết quả theo số lượng khách hàng giảm dần, sau đó theo `category1` tăng dần rồi đến `category2` tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    user_category AS (
        SELECT DISTINCT
            user_id,
            category
        FROM
            ProductPurchases
            JOIN ProductInfo USING (product_id)
    ),
    pair_per_user AS (
        SELECT
            a.user_id,
            a.category AS category1,
            b.category AS category2
        FROM
            user_category AS a
            JOIN user_category AS b ON a.user_id = b.user_id AND a.category < b.category
    )
SELECT category1, category2, COUNT(DISTINCT user_id) AS customer_count
FROM pair_per_user
GROUP BY 1, 2
HAVING customer_count >= 3
ORDER BY 3 DESC, 1, 2;
```

#### Pandas

```python
import pandas as pd


def find_category_recommendation_pairs(
    product_purchases: pd.DataFrame, product_info: pd.DataFrame
) -> pd.DataFrame:
    df = product_purchases[["user_id", "product_id"]].merge(
        product_info[["product_id", "category"]], on="product_id", how="inner"
    )
    user_category = df.drop_duplicates(subset=["user_id", "category"])
    pair_per_user = (
        user_category.merge(user_category, on="user_id")
        .query("category_x < category_y")
        .rename(columns={"category_x": "category1", "category_y": "category2"})
    )
    pair_counts = (
        pair_per_user.groupby(["category1", "category2"])["user_id"]
        .nunique()
        .reset_index(name="customer_count")
    )
    result = (
        pair_counts.query("customer_count >= 3")
        .sort_values(
            ["customer_count", "category1", "category2"], ascending=[False, True, True]
        )
        .reset_index(drop=True)
    )
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
