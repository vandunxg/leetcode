---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3521. Find Product Recommendation Pairs](https://leetcode.com/problems/find-product-recommendation-pairs)

[中文文档](/solution/3500-3599/3521.Find%20Product%20Recommendation%20Pairs/README.md)

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
+------------+---------+
| Column Name | Type    |
+------------+---------+
| product_id  | int     |
| category    | varchar |
| price       | decimal |
+------------+---------+
product_id là khóa chính của bảng này.
Mỗi hàng gán một danh mục và giá cho một sản phẩm.
</pre>

<p>Amazon muốn triển khai tính năng <strong>Khách hàng mua sản phẩm này cũng mua...</strong> dựa trên <strong>các mô hình mua chung</strong>. Hãy viết lời giải để:</p>

<ol>
    <li>Xác định các cặp sản phẩm <strong>khác nhau</strong> thường xuyên được <strong>cùng một khách hàng mua cùng nhau</strong> (trong đó <code>product1_id</code> &lt; <code>product2_id</code>)</li>
    <li>Với <strong>mỗi cặp sản phẩm</strong>, xác định có bao nhiêu khách hàng đã mua <strong>cả hai</strong> sản phẩm</li>
</ol>

<p><strong>Một cặp sản phẩm </strong>được xem xét để đề xuất <strong>nếu</strong> <strong>ít nhất</strong> có <code>3</code> khách hàng <strong>khác nhau</strong> đã mua <strong>cả hai sản phẩm</strong>.</p>

<p><em>Trả về </em><em>bảng kết quả được sắp xếp theo <strong>customer_count</strong> theo <strong>thứ tự giảm dần</strong>, nếu có nhiều kết quả bằng nhau, theo </em><code>product1_id</code><em> theo <strong>thứ tự tăng dần</strong>, sau đó theo </em><code>product2_id</code><em> theo <strong>thứ tự tăng dần</strong></em>.</p>

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
| 1       | 103        | 3        |
| 2       | 101        | 1        |
| 2       | 102        | 5        |
| 2       | 104        | 1        |
| 3       | 101        | 2        |
| 3       | 103        | 1        |
| 3       | 105        | 4        |
| 4       | 101        | 1        |
| 4       | 102        | 1        |
| 4       | 103        | 2        |
| 4       | 104        | 3        |
| 5       | 102        | 2        |
| 5       | 104        | 1        |
+---------+------------+----------+
</pre>

<p>Bảng ProductInfo:</p>

<pre class="example-io">
+------------+-------------+-------+
| product_id | category    | price |
+------------+-------------+-------+
| 101        | Electronics | 100   |
| 102        | Books       | 20    |
| 103        | Clothing    | 35    |
| 104        | Kitchen     | 50    |
| 105        | Sports      | 75    |
+------------+-------------+-------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+-------------+-------------------+-------------------+----------------+
| product1_id | product2_id | product1_category | product2_category | customer_count |
+-------------+-------------+-------------------+-------------------+----------------+
| 101         | 102         | Electronics       | Books             | 3              |
| 101         | 103         | Electronics       | Clothing          | 3              |
| 102         | 104         | Books             | Kitchen           | 3              |
+-------------+-------------+-------------------+-------------------+----------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Cặp sản phẩm (101, 102):</strong>

    <ul>
        <li>Được người dùng 1, 2 và 4 mua (3 khách hàng)</li>
        <li>Sản phẩm 101 thuộc danh mục Electronics</li>
        <li>Sản phẩm 102 thuộc danh mục Books</li>
    </ul>
    </li>
    <li><strong>Cặp sản phẩm (101, 103):</strong>
    <ul>
        <li>Được người dùng 1, 3 và 4 mua (3 khách hàng)</li>
        <li>Sản phẩm 101 thuộc danh mục Electronics</li>
        <li>Sản phẩm 103 thuộc danh mục Clothing</li>
    </ul>
    </li>
    <li><strong>Cặp sản phẩm (102, 104):</strong>
    <ul>
        <li>Được người dùng 2, 4 và 5 mua (3 khách hàng)</li>
        <li>Sản phẩm 102 thuộc danh mục Books</li>
        <li>Sản phẩm 104 thuộc danh mục Kitchen</li>
    </ul>
    </li>

</ul>

<p>Kết quả được sắp xếp theo customer_count theo thứ tự giảm dần. Với các cặp có cùng customer_count, chúng được sắp xếp theo product1_id rồi đến product2_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm các cặp sản phẩm được ít nhất ba người dùng mua cùng nhau. Loại bỏ các hàng user-product trùng lặp rồi self-join theo $\textit{user\_id}$ để tạo các cặp theo thứ tự.
>
> Gom nhóm số người dùng khác nhau theo từng cặp, join thông tin danh mục, rồi sắp xếp theo yêu cầu. Phép equi-join kết hợp với grouping thay thế cho ba vòng lặp lồng nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
