---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2252. Dynamic Pivoting of a Table 🔒](https://leetcode.com/problems/dynamic-pivoting-of-a-table)

[中文文档](/solution/2200-2299/2252.Dynamic%20Pivoting%20of%20a%20Table/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Products</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| product_id  | int     |
| store       | varchar |
| price       | int     |
+-------------+---------+
(product_id, store) là khóa chính (tổ hợp các cột có giá trị không trùng nhau) của bảng này.
Mỗi hàng của bảng này cho biết giá của product_id tại store.
Bảng có nhiều nhất 30 store khác nhau.
price là giá của sản phẩm tại store này.
</pre>

<p>&nbsp;</p>

<p><strong>Lưu ý quan trọng:</strong> Bài toán này dành cho những người có nhiều kinh nghiệm với SQL. Nếu bạn là người mới bắt đầu, chúng tôi khuyến nghị bạn tạm thời bỏ qua bài này.</p>

<p>Hãy cài đặt procedure <code>PivotProducts</code> để tổ chức lại bảng <code>Products</code> sao cho mỗi hàng chứa ID của một sản phẩm và giá của sản phẩm đó tại từng store. Giá phải là <code>null</code> nếu sản phẩm không được bán tại một store. Các cột của bảng phải chứa từng store và được sắp xếp theo <strong>thứ tự từ điển</strong>.</p>

<p>Procedure phải trả về bảng sau khi tổ chức lại.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Products:
+------------+----------+-------+
| product_id | store    | price |
+------------+----------+-------+
| 1          | Shop     | 110   |
| 1          | LC_Store | 100   |
| 2          | Nozama   | 200   |
| 2          | Souq     | 190   |
| 3          | Shop     | 1000  |
| 3          | Souq     | 1900  |
+------------+----------+-------+
<strong>Đầu ra:</strong>
+------------+----------+--------+------+------+
| product_id | LC_Store | Nozama | Shop | Souq |
+------------+----------+--------+------+------+
| 1          | 100      | null   | 110  | null |
| 2          | null     | 200    | null | 190  |
| 3          | null     | null   | 1000 | 1900 |
+------------+----------+--------+------+------+
<strong>Giải thích:</strong>
Có 4 store: Shop, LC_Store, Nozama và Souq. Trước tiên, ta sắp xếp chúng theo thứ tự từ điển: LC_Store, Nozama, Shop và Souq.
Với sản phẩm 1, giá tại LC_Store là 100 và tại Shop là 110. Sản phẩm không được bán tại hai store còn lại nên giá ở đó là null.
Tương tự, sản phẩm 2 có giá 200 tại Nozama và 190 tại Souq. Sản phẩm không được bán tại hai store còn lại.
Với sản phẩm 3, giá tại Shop là 1000 và tại Souq là 1900. Sản phẩm không được bán tại hai store còn lại.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tên store không được biết trước; ta phải pivot $(\textit{product\_id}, \textit{store}, \textit{price})$ thành một cột cho mỗi store. Các biểu thức hard-code với $\textit{CASE WHEN}$ không thể đặt tên cho những store chưa biết, vì vậy danh sách cột phải được xây dựng từ dữ liệu.
>
> $\textit{GROUP\_CONCAT}$ tạo ra các phần $\textit{MAX}(\textit{CASE WHEN store}=\ldots)$, được bọc trong $\textit{SELECT product\_id},\ldots\ \textit{GROUP BY product\_id}$, sau đó được prepare và execute. Tăng $\textit{group\_concat\_max\_len}$ để tên không bị cắt ngắn.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
CREATE PROCEDURE PivotProducts()
BEGIN
	# Write your MySQL query statement below.
	SET group_concat_max_len = 5000;
    SELECT GROUP_CONCAT(DISTINCT 'MAX(CASE WHEN store = \'',
               store,
               '\' THEN price ELSE NULL END) AS ',
               store
               ORDER BY store) INTO @sql
    FROM Products;
    SET @sql =  CONCAT('SELECT product_id, ',
                    @sql,
                    ' FROM Products GROUP BY product_id');
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
