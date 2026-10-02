---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price)

[中文文档](/solution/1200-1299/1251.Average%20Selling%20Price/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Prices</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |
+---------------+---------+
(product_id, start_date, end_date) là khóa chính của bảng này (tổ hợp các cột có giá trị duy nhất).
Mỗi hàng trong bảng cho biết giá của product_id trong khoảng thời gian từ start_date đến end_date.
Với mỗi product_id, không có hai khoảng thời gian nào bị chồng lấn, tức là không có hai khoảng giao nhau cho cùng một product_id.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>UnitsSold</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| purchase_date | date    |
| units         | int     |
+---------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Mỗi hàng trong bảng cho biết ngày bán, số lượng và product_id của sản phẩm được bán. 
</pre>

<p>&nbsp;</p>

<p>Viết truy vấn tìm giá bán trung bình của mỗi sản phẩm. <code>average_price</code> cần được <strong>làm tròn đến 2 chữ số thập phân</strong>. Nếu sản phẩm không bán được đơn vị nào, giá bán trung bình được xem là 0.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Prices:
+------------+------------+------------+--------+
| product_id | start_date | end_date   | price  |
+------------+------------+------------+--------+
| 1          | 2019-02-17 | 2019-02-28 | 5      |
| 1          | 2019-03-01 | 2019-03-22 | 20     |
| 2          | 2019-02-01 | 2019-02-20 | 15     |
| 2          | 2019-02-21 | 2019-03-31 | 30     |
+------------+------------+------------+--------+
Bảng UnitsSold:
+------------+---------------+-------+
| product_id | purchase_date | units |
+------------+---------------+-------+
| 1          | 2019-02-25    | 100   |
| 1          | 2019-03-01    | 15    |
| 2          | 2019-02-10    | 200   |
| 2          | 2019-03-22    | 30    |
+------------+---------------+-------+
<strong>Đầu ra:</strong> 
+------------+---------------+
| product_id | average_price |
+------------+---------------+
| 1          | 6.96          |
| 2          | 16.96         |
+------------+---------------+
<strong>Giải thích:</strong> 
Giá bán trung bình = Tổng giá trị sản phẩm / Số lượng sản phẩm đã bán.
Giá bán trung bình của sản phẩm 1 = ((100 * 5) + (15 * 20)) / 115 = 6.96
Giá bán trung bình của sản phẩm 2 = ((200 * 15) + (30 * 30)) / 230 = 16.96
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Left Join + Grouping

<!-- thinking:start -->

> **Tư duy**
>
> Giá trung bình được tính bằng $\sum price\times units / \sum units$, còn giá sản phẩm có hiệu lực theo từng khoảng ngày. Dùng left join $Prices$ với $UnitsSold$, chỉ ghép các giao dịch có ngày nằm trong $[start,end]$ của sản phẩm tương ứng, đồng thời vẫn giữ lại sản phẩm chưa có lượt bán.
>
> Sau khi nhóm theo sản phẩm, $IFNULL$ chuyển giá trị trung bình có trọng số null thành $0$. Điều kiện join giới hạn khoảng ngày; phép nhóm tính giá trung bình có trọng số.

<!-- thinking:end -->

Ta có thể dùng left join để nối bảng `Prices` với bảng `UnitsSold` theo `product_id`, đồng thời yêu cầu `purchase_date` nằm giữa `start_date` và `end_date`. Sau đó, dùng `GROUP BY` nhóm theo `product_id` để tổng hợp và dùng hàm `AVG` tính giá trung bình. Lưu ý, nếu sản phẩm không có bản ghi bán hàng thì `AVG` trả về `NULL`; ta có thể dùng `IFNULL` để chuyển giá trị đó thành $0$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    p.product_id,
    IFNULL(ROUND(SUM(price * units) / SUM(units), 2), 0) AS average_price
FROM
    Prices AS p
    LEFT JOIN UnitsSold AS u
        ON p.product_id = u.product_id AND purchase_date BETWEEN start_date AND end_date
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
