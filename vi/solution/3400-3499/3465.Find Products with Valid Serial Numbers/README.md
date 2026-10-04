---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3465. Find Products with Valid Serial Numbers](https://leetcode.com/problems/find-products-with-valid-serial-numbers)

[中文文档](/solution/3400-3499/3465.Find%20Products%20with%20Valid%20Serial%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>products</code></p>

<pre>
+--------------+------------+
| Column Name  | Type       |
+--------------+------------+
| product_id   | int        |
| product_name | varchar    |
| description  | varchar    |
+--------------+------------+
(product_id) là khóa duy nhất của bảng này.
Mỗi hàng trong bảng đại diện cho một sản phẩm với ID, tên và description duy nhất.
</pre>

<p>Viết lời giải để tìm tất cả sản phẩm có description <strong>chứa pattern serial number hợp lệ</strong>. Một serial number hợp lệ tuân theo các quy tắc sau:</p>

<ul>
	<li>Bắt đầu bằng hai chữ cái <strong>SN</strong>&nbsp;(phân biệt chữ hoa, chữ thường).</li>
	<li>Tiếp theo là đúng <code>4</code> chữ số.</li>
	<li>Phải có dấu gạch nối (-) <strong>theo sau là đúng</strong> <code>4</code> chữ số.</li>
	<li>Serial number phải nằm bên trong description (không nhất thiết phải bắt đầu từ đầu).</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>product_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng products:</p>

<pre class="example-io">
+------------+--------------+------------------------------------------------------+
| product_id | product_name | description                                          |
+------------+--------------+------------------------------------------------------+
| 1          | Widget A     | This is a sample product with SN1234-5678            |
| 2          | Widget B     | A product with serial SN9876-1234 in the description |
| 3          | Widget C     | Product SN1234-56789 is available now                |
| 4          | Widget D     | No serial number here                                |
| 5          | Widget E     | Check out SN4321-8765 in this description            |
+------------+--------------+------------------------------------------------------+
    </pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------+--------------+------------------------------------------------------+
| product_id | product_name | description                                          |
+------------+--------------+------------------------------------------------------+
| 1          | Widget A     | This is a sample product with SN1234-5678            |
| 2          | Widget B     | A product with serial SN9876-1234 in the description |
| 5          | Widget E     | Check out SN4321-8765 in this description            |
+------------+--------------+------------------------------------------------------+
    </pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Sản phẩm 1:</strong> Serial number hợp lệ SN1234-5678</li>
	<li><strong>Sản phẩm 2:</strong> Serial number hợp lệ SN9876-1234</li>
	<li><strong>Sản phẩm 3:</strong> Serial number không hợp lệ SN1234-56789 (có 5 chữ số sau dấu gạch nối)</li>
	<li><strong>Sản phẩm 4:</strong> Không có serial number trong description</li>
	<li><strong>Sản phẩm 5:</strong> Serial number hợp lệ SN4321-8765</li>
</ul>

<p>Bảng kết quả được sắp xếp theo product_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So khớp bằng Regex

<!-- thinking:start -->

> **Tư duy**
>
> Một description phải chứa serial có dạng $\textit{SN}$, bốn chữ số, một dấu gạch nối và thêm bốn chữ số. Tìm kiếm substring trực tiếp có thể nối pattern đó với các ký tự chữ hoặc số ở hai bên.
>
> Word boundary $\b$ giúp serial trở thành một token độc lập.
>
> Lọc bằng `\bSN[0-9]{4}-[0-9]{4}\b` và sắp xếp theo $\textit{product\_id}$.

<!-- thinking:end -->

Theo đề bài, chúng ta cần tìm tất cả sản phẩm chứa serial number hợp lệ, với các quy tắc của serial number hợp lệ như sau:

- Bắt đầu bằng `SN` (phân biệt chữ hoa, chữ thường).
- Tiếp theo là 4 chữ số.
- Phải có dấu gạch nối `-`, theo sau là 4 chữ số.

Dựa trên các quy tắc trên, chúng ta có thể sử dụng regular expression để khớp các serial number hợp lệ, sau đó lọc những sản phẩm chứa serial number hợp lệ và cuối cùng sắp xếp chúng theo thứ tự tăng dần của `product_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT product_id, product_name, description
FROM products
WHERE description REGEXP '(?-i)\\bSN[0-9]{4}-[0-9]{4}\\b'
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def find_valid_serial_products(products: pd.DataFrame) -> pd.DataFrame:
    valid_pattern = r"\bSN[0-9]{4}-[0-9]{4}\b"
    valid_products = products[
        products["description"].str.contains(valid_pattern, regex=True)
    ]
    valid_products = valid_products.sort_values(by="product_id")
    return valid_products
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
