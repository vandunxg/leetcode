---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3415. Find Products with Three Consecutive Digits 🔒](https://leetcode.com/problems/find-products-with-three-consecutive-digits)

[中文文档](/solution/3400-3499/3415.Find%20Products%20with%20Three%20Consecutive%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Products</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| name        | varchar |
+-------------+---------+
product_id là khóa duy nhất của bảng này.
Mỗi hàng của bảng này chứa ID và tên của một sản phẩm.
</pre>

<p>Viết lời giải để tìm tất cả <strong>sản phẩm</strong> có tên chứa <strong>một chuỗi gồm đúng ba chữ số liên tiếp</strong>.&nbsp;</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>product_id</code> <em>theo <strong>thứ tự tăng dần</strong>.</em></p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p><strong>Lưu ý</strong> rằng tên có thể chứa nhiều chuỗi như vậy, nhưng mỗi chuỗi phải có độ dài bằng ba.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng products:</p>

<pre class="example-io">
+-------------+--------------------+
| product_id  | name               |
+-------------+--------------------+
| 1           | ABC123XYZ          |
| 2           | A12B34C            |
| 3           | Product56789       |
| 4           | NoDigitsHere       |
| 5           | 789Product         |
| 6           | Item003Description |
| 7           | Product12X34       |
+-------------+--------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+--------------------+
| product_id  | name               |
+-------------+--------------------+
| 1           | ABC123XYZ          |
| 5           | 789Product         |
| 6           | Item003Description |
+-------------+--------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sản phẩm 1: ABC123XYZ chứa các chữ số 123.</li>
	<li>Sản phẩm 5: 789Product&nbsp;chứa các chữ số 789.</li>
	<li>Sản phẩm 6: Item003Description&nbsp;chứa 003, đây chính xác là ba chữ số.</li>
</ul>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Kết quả được sắp xếp theo <code>product_id</code> theo thứ tự tăng dần.</li>
	<li>Chỉ những sản phẩm có đúng ba chữ số liên tiếp trong tên mới được đưa vào kết quả.</li>
</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So khớp bằng Regex

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần tìm những sản phẩm có tên chứa ba chữ số liên tiếp. Tự viết vòng lặp để quét chuỗi rất dễ bỏ sót ranh giới giữa chữ số và chữ cái hoặc vị trí cuối chuỗi.
>
> Một regular expression duy nhất có thể xử lý các trường hợp đó. Mẫu $(^|[^0-9])[0-9]{3}([^0-9]|$)$ khớp một đoạn gồm ba chữ số độc lập, không coi một đoạn nhiều chữ số là nhiều kết quả chồng lấn ngoài ý muốn.
>
> Chúng ta lọc bảng bằng mẫu đó và sắp xếp theo $\textit{product\_id}$ để khớp với thứ tự được yêu cầu.

<!-- thinking:end -->

Chúng ta có thể dùng regular expression để so khớp tên sản phẩm có chứa ba chữ số liên tiếp.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT product_id, name
FROM Products
WHERE name REGEXP '(^|[^0-9])[0-9]{3}([^0-9]|$)'
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def find_products(products: pd.DataFrame) -> pd.DataFrame:
    filtered = products[
        products["name"].str.contains(r"(^|[^0-9])[0-9]{3}([^0-9]|$)", regex=True)
    ]
    return filtered.sort_values(by="product_id")
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
