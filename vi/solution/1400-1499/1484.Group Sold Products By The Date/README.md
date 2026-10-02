---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1484. Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date)

[中文文档](/solution/1400-1499/1484.Group%20Sold%20Products%20By%20The%20Date/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng <code>Activities</code>:</p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| sell_date   | date    |
| product     | varchar |
+-------------+---------+
Bảng này không có khóa chính (cột chứa các giá trị duy nhất). Bảng có thể chứa các bản ghi trùng lặp.
Mỗi hàng trong bảng chứa tên sản phẩm và ngày sản phẩm đó được bán tại một khu chợ.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm số lượng sản phẩm khác nhau được bán và tên của chúng cho từng ngày.</p>

<p>Tên các sản phẩm được bán trong mỗi ngày phải được sắp xếp theo thứ tự từ điển.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>sell_date</code>.</p>

<p>&nbsp;Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Activities:
+------------+------------+
| sell_date  | product     |
+------------+------------+
| 2020-05-30 | Headphone  |
| 2020-06-01 | Pencil     |
| 2020-06-02 | Mask       |
| 2020-05-30 | Basketball |
| 2020-06-01 | Bible      |
| 2020-06-02 | Mask       |
| 2020-05-30 | T-Shirt    |
+------------+------------+
<strong>Đầu ra:</strong>
+------------+----------+------------------------------+
| sell_date  | num_sold | products                     |
+------------+----------+------------------------------+
| 2020-05-30 | 3        | Basketball,Headphone,T-shirt |
| 2020-06-01 | 2        | Bible,Pencil                 |
| 2020-06-02 | 1        | Mask                         |
+------------+----------+------------------------------+
<strong>Giải thích:</strong>
Với ngày 2020-05-30, các sản phẩm được bán là (Headphone, Basketball, T-shirt), ta sắp xếp chúng theo thứ tự từ điển và ngăn cách bằng dấu phẩy.
Với ngày 2020-06-01, các sản phẩm được bán là (Pencil, Bible), ta sắp xếp chúng theo thứ tự từ điển và ngăn cách bằng dấu phẩy.
Với ngày 2020-06-02, sản phẩm được bán là (Mask), ta chỉ cần trả về sản phẩm đó.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi ngày, ta cần số lượng sản phẩm khác nhau và danh sách tên được sắp xếp. `GROUP BY sell_date` cùng với `COUNT(DISTINCT product)` và `GROUP_CONCAT(DISTINCT product)` sẽ tạo ra cả hai kết quả.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    sell_date,
    COUNT(DISTINCT product) AS num_sold,
    GROUP_CONCAT(DISTINCT product) AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
