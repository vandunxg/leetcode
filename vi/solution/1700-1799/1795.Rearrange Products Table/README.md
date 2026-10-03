---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1795. Rearrange Products Table](https://leetcode.com/problems/rearrange-products-table)

[中文文档](/solution/1700-1799/1795.Rearrange%20Products%20Table/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Products</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| store1      | int     |
| store2      | int     |
| store3      | int     |
+-------------+---------+
product_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết giá của sản phẩm tại 3 cửa hàng khác nhau: store1, store2 và store3.
Nếu sản phẩm không có tại một cửa hàng, cột của cửa hàng đó sẽ có giá trị null.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải sắp xếp lại bảng <code>Products</code> sao cho mỗi hàng có dạng <code>(product_id, store, price)</code>. Nếu sản phẩm không có tại một cửa hàng, <strong>không</strong> đưa hàng có cặp <code>product_id</code> và <code>store</code> tương ứng vào bảng kết quả.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Products:
+------------+--------+--------+--------+
| product_id | store1 | store2 | store3 |
+------------+--------+--------+--------+
| 0          | 95     | 100    | 105    |
| 1          | 70     | null   | 80     |
+------------+--------+--------+--------+
<strong>Đầu ra:</strong>
+------------+--------+-------+
| product_id | store  | price |
+------------+--------+-------+
| 0          | store1 | 95    |
| 0          | store2 | 100   |
| 0          | store3 | 105   |
| 1          | store1 | 70    |
| 1          | store3 | 80    |
+------------+--------+-------+
<strong>Giải thích:</strong>
Sản phẩm 0 có mặt tại cả ba cửa hàng với giá lần lượt là 95, 100 và 105.
Sản phẩm 1 có mặt tại store1 với giá 70 và store3 với giá 80. Sản phẩm không có tại store2.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phép UNION

<!-- thinking:start -->

> **Tư duy**
>
> Ba cột giá theo cửa hàng cần được chuyển thành các hàng dạng dài $(\textit{product},\textit{store},\textit{price})$. Giá null nghĩa là cửa hàng không bán sản phẩm đó.
>
> Chọn các hàng không rỗng của từng cửa hàng, gắn tên cửa hàng rồi $\mathrm{UNION}$ chúng.

<!-- thinking:end -->

Ta có thể chọn sản phẩm và giá của từng cửa hàng, sau đó dùng toán tử `UNION` để gộp các kết quả.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT product_id, 'store1' AS store, store1 AS price FROM Products WHERE store1 IS NOT NULL
UNION
SELECT product_id, 'store2' AS store, store2 AS price FROM Products WHERE store2 IS NOT NULL
UNION
SELECT product_id, 'store3' AS store, store3 AS price FROM Products WHERE store3 IS NOT NULL;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
