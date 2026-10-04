---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2985. Calculate Compressed Mean 🔒](https://leetcode.com/problems/calculate-compressed-mean)

[中文文档](/solution/2900-2999/2985.Calculate%20Compressed%20Mean/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Orders</code></p>

<pre>
+-------------------+------+
| Column Name       | Type |
+-------------------+------+
| order_id          | int  |
| item_count        | int  |
| order_occurrences | int  |
+-------------------+------+
order_id là cột chứa các giá trị duy nhất trong bảng này.
Bảng này chứa order_id, item_count và order_occurrences.
</pre>

<p>Hãy viết lời giải để tính <strong>trung bình</strong> số lượng item trên mỗi order, làm tròn đến <code>2</code> <strong>chữ số thập phân</strong>.</p>

<p><em>Trả về bảng kết quả</em><em> theo <strong>bất kỳ</strong> thứ tự nào</em><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Orders:
+----------+------------+-------------------+
| order_id | item_count | order_occurrences |
+----------+------------+-------------------+
| 10       | 1          | 500               |
| 11       | 2          | 1000              |
| 12       | 3          | 800               |
| 13       | 4          | 1000              |
+----------+------------+-------------------+
<strong>Đầu ra</strong>
+-------------------------+
| average_items_per_order |
+-------------------------+
| 2.70                    |
+-------------------------+
<strong>Giải thích</strong>
Phép tính được thực hiện như sau:
 - Tổng số item: (1 * 500) + (2 * 1000) + (3 * 800) + (4 * 1000) = 8900
 - Tổng số order: 500 + 1000 + 800 + 1000 = 3300
 - Do đó, số item trung bình trên mỗi order là 8900 / 3300 = 2.70</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Bảng đã lưu số lượng item nhân với tần suất xuất hiện, nên giá trị trung bình là tỷ số giữa hai tổng này. Chỉ cần một cặp $SUM$ và $ROUND$ để làm tròn đến hai chữ số thập phân.
>
> Không cần tách các order thành các dòng riêng.

<!-- thinking:end -->

Ta dùng hàm `SUM` để tính tổng số lượng sản phẩm và tổng số order, sau đó chia tổng số lượng cho tổng số order để tính giá trị trung bình. Cuối cùng, dùng hàm `ROUND` để làm tròn kết quả đến hai chữ số thập phân.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    ROUND(
        SUM(item_count * order_occurrences) / SUM(order_occurrences),
        2
    ) AS average_items_per_order
FROM Orders;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
