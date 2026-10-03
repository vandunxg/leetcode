---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2292. Products With Three or More Orders in Two Consecutive Years 🔒](https://leetcode.com/problems/products-with-three-or-more-orders-in-two-consecutive-years)

[中文文档](/solution/2200-2299/2292.Products%20With%20Three%20or%20More%20Orders%20in%20Two%20Consecutive%20Years/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Orders</code></p>

<pre>
+---------------+------+
| Column Name   | Type |
+---------------+------+
| order_id      | int  |
| product_id    | int  |
| quantity      | int  |
| purchase_date | date |
+---------------+------+
order_id chứa các giá trị không trùng nhau.
Mỗi dòng trong bảng này chứa ID của đơn hàng, ID của sản phẩm được mua, số lượng và ngày mua.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm ID của tất cả sản phẩm được đặt hàng từ ba lần trở lên trong hai năm liên tiếp.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Orders:
+----------+------------+----------+---------------+
| order_id | product_id | quantity | purchase_date |
+----------+------------+----------+---------------+
| 1        | 1          | 7        | 2020-03-16    |
| 2        | 1          | 4        | 2020-12-02    |
| 3        | 1          | 7        | 2020-05-10    |
| 4        | 1          | 6        | 2021-12-23    |
| 5        | 1          | 5        | 2021-05-21    |
| 6        | 1          | 6        | 2021-10-11    |
| 7        | 2          | 6        | 2022-10-11    |
+----------+------------+----------+---------------+
<strong>Đầu ra:</strong>
+------------+
| product_id |
+------------+
| 1          |
+------------+
<strong>Giải thích:</strong>
Sản phẩm 1 được đặt hàng ba lần vào năm 2020 và ba lần vào năm 2021. Vì sản phẩm này được đặt hàng ba lần trong hai năm liên tiếp, ta đưa sản phẩm vào kết quả.
Sản phẩm 2 được đặt hàng một lần vào năm 2022. Ta không đưa sản phẩm này vào kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm những sản phẩm có ít nhất ba đơn hàng trong hai năm liên tiếp. Sau khi nhóm theo sản phẩm và năm, ta kiểm tra các năm liền kề. Self-join là cách trực tiếp: đánh dấu liệu một năm có $\ge 3$ đơn hàng hay không, sau đó join các dòng cách nhau một năm và đều có dấu là đúng.
>
> CTE tổng hợp $(\textit{product\_id}, \textit{YEAR})$ thành $\textit{mark}$; phép join dùng điều kiện $p_1.y = p_2.y-1$ trên cùng một sản phẩm với cả hai dấu đều được bật, sau đó lấy các ID không trùng nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT product_id, YEAR(purchase_date) AS y, COUNT(1) >= 3 AS mark
        FROM Orders
        GROUP BY 1, 2
    )
SELECT DISTINCT p1.product_id
FROM
    P AS p1
    JOIN P AS p2 ON p1.y = p2.y - 1 AND p1.product_id = p2.product_id
WHERE p1.mark AND p2.mark;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 giữ lại mọi năm rồi lọc theo $\textit{mark}$. Thay vào đó, ta chỉ giữ các năm có $\textit{HAVING COUNT}(1)\ge 3$, nên self-join không cần thêm điều kiện lọc. Kết quả thu được là tương tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT product_id, YEAR(purchase_date) AS y
        FROM Orders
        GROUP BY 1, 2
        HAVING COUNT(1) >= 3
    )
SELECT DISTINCT p1.product_id
FROM
    P AS p1
    JOIN P AS p2 ON p1.y = p2.y - 1 AND p1.product_id = p2.product_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
