---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2253. Dynamic Unpivoting of a Table 🔒](https://leetcode.com/problems/dynamic-unpivoting-of-a-table)

[中文文档](/solution/2200-2299/2253.Dynamic%20Unpivoting%20of%20a%20Table/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Products</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| product_id  | int     |
| store_name<sub>1</sub> | int     |
| store_name<sub>2</sub> | int     |
|      :      | int     |
|      :      | int     |
|      :      | int     |
| store_name<sub>n</sub> | int     |
+-------------+---------+
product_id là khóa chính của bảng này.
Mỗi hàng trong bảng này cho biết giá của sản phẩm tại n cửa hàng khác nhau.
Nếu sản phẩm không có sẵn tại một cửa hàng, giá ở cột của cửa hàng đó sẽ là null.
Tên các cửa hàng có thể thay đổi giữa các test case. Có ít nhất 1 cửa hàng và nhiều nhất 30 cửa hàng.
</pre>

<p>&nbsp;</p>

<p><strong>Lưu ý quan trọng:</strong> Bài toán này dành cho những người có kinh nghiệm tốt với SQL. Nếu bạn là người mới bắt đầu, chúng tôi khuyên bạn nên tạm bỏ qua bài này.</p>

<p>Hãy cài đặt procedure <code>UnpivotProducts</code> để tổ chức lại bảng <code>Products</code> sao cho mỗi hàng chứa ID của một sản phẩm, tên của một cửa hàng bán sản phẩm đó và giá tại cửa hàng đó. Nếu sản phẩm không có sẵn tại một cửa hàng, <strong>không</strong> đưa hàng chứa tổ hợp <code>product_id</code> và <code>store</code> tương ứng vào bảng kết quả. Bảng kết quả gồm ba cột: <code>product_id</code>, <code>store</code> và <code>price</code>.</p>

<p>Procedure phải trả về bảng sau khi đã tổ chức lại.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Products:
+------------+----------+--------+------+------+
| product_id | LC_Store | Nozama | Shop | Souq |
+------------+----------+--------+------+------+
| 1          | 100      | null   | 110  | null |
| 2          | null     | 200    | null | 190  |
| 3          | null     | null   | 1000 | 1900 |
+------------+----------+--------+------+------+
<strong>Đầu ra:</strong>
+------------+----------+-------+
| product_id | store    | price |
+------------+----------+-------+
| 1          | LC_Store | 100   |
| 1          | Shop     | 110   |
| 2          | Nozama   | 200   |
| 2          | Souq     | 190   |
| 3          | Shop     | 1000  |
| 3          | Souq     | 1900  |
+------------+----------+-------+
<strong>Giải thích:</strong>
Sản phẩm 1 được bán tại LC_Store và Shop với giá lần lượt là 100 và 110.
Sản phẩm 2 được bán tại Nozama và Souq với giá lần lượt là 200 và 190.
Sản phẩm 3 được bán tại Shop và Souq với giá lần lượt là 1000 và 1900.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đây là phép đảo ngược của bài trước: tên cửa hàng là các cột và phải trở thành các hàng $(\textit{product\_id}, \textit{store}, \textit{price})$, đồng thời loại bỏ các giá trị null. Tên các cột không được biết trước, nên chúng được lấy từ $\textit{information\_schema.columns}$.
>
> Mỗi cột không phải là $\textit{product\_id}$ trở thành một câu lệnh $\textit{SELECT}$ ghi lại tên cửa hàng và lọc các giá trị không phải null; $\textit{UNION}$ nối chúng thành một prepared statement.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
CREATE PROCEDURE UnpivotProducts()
BEGIN
    # Write your MySQL query statement below.
    SET group_concat_max_len = 5000;
    WITH
        t AS (
            SELECT column_name
            FROM information_schema.columns
            WHERE
                table_schema = DATABASE()
                AND table_name = 'Products'
                AND column_name != 'product_id'
        )
    SELECT
        GROUP_CONCAT(
            'SELECT product_id, \'',
            column_name,
            '\' store, ',
            column_name,
            ' price FROM Products WHERE ',
            column_name,
            ' IS NOT NULL' SEPARATOR ' UNION '
        ) INTO @sql from t;
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
