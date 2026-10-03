---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products)

[中文文档](/solution/1700-1799/1757.Recyclable%20and%20Low%20Fat%20Products/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Products</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |
+-------------+---------+
product_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
low_fats là một ENUM (danh mục) có kiểu (&#39;Y&#39;, &#39;N&#39;), trong đó &#39;Y&#39; nghĩa là sản phẩm ít chất béo và &#39;N&#39; nghĩa là không.
recyclable là một ENUM (danh mục) có kiểu (&#39;Y&#39;, &#39;N&#39;), trong đó &#39;Y&#39; nghĩa là sản phẩm có thể tái chế và &#39;N&#39; nghĩa là không.</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm mã của các sản phẩm vừa ít chất béo vừa có thể tái chế.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Products:
+-------------+----------+------------+
| product_id  | low_fats | recyclable |
+-------------+----------+------------+
| 0           | Y        | N          |
| 1           | Y        | Y          |
| 2           | N        | Y          |
| 3           | Y        | Y          |
| 4           | N        | N          |
+-------------+----------+------------+
<strong>Đầu ra:</strong>
+-------------+
| product_id  |
+-------------+
| 1           |
| 3           |
+-------------+
<strong>Giải thích:</strong> Chỉ các sản phẩm 1 và 3 vừa ít chất béo vừa có thể tái chế.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Conditional Filtering

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần các mã sản phẩm vừa ít chất béo vừa có thể tái chế.
>
> Lọc các hàng mà cả $\textit{low\_fats}$ và $\textit{recyclable}$ đều là $Y$, sau đó lấy cột $\textit{product\_id}$.

<!-- thinking:end -->

Ta có thể trực tiếp lọc các mã sản phẩm có `low_fats` là `Y` và `recyclable` là `Y`.

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def find_products(products: pd.DataFrame) -> pd.DataFrame:
    rs = products[(products["low_fats"] == "Y") & (products["recyclable"] == "Y")]
    rs = rs[["product_id"]]
    return rs
```

#### MySQL

```sql
SELECT
    product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
