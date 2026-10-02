---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii)

[中文文档](/solution/1100-1199/1174.Immediate%20Food%20Delivery%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Delivery</code></p>

<pre>
+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+
delivery_id là cột có giá trị duy nhất trong bảng này.
Bảng lưu thông tin giao đồ ăn cho khách hàng, gồm ngày đặt hàng và ngày giao hàng mong muốn (cùng ngày đặt hàng hoặc sau đó).
</pre>

<p>&nbsp;</p>

<p>Nếu ngày giao hàng mong muốn của khách trùng với ngày đặt hàng, đơn được gọi là <strong>giao ngay</strong>; nếu không, đó là đơn <strong>đặt lịch</strong>.</p>

<p><strong>Đơn hàng đầu tiên</strong> của một khách hàng là đơn có ngày đặt hàng sớm nhất. Đảm bảo mỗi khách hàng có chính xác một đơn hàng đầu tiên.</p>

<p>Hãy tìm tỷ lệ phần trăm đơn giao ngay trong số đơn hàng đầu tiên của tất cả khách hàng, <strong>làm tròn đến 2 chữ số thập phân</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Delivery:
+-------------+-------------+------------+-----------------------------+
| delivery_id | customer_id | order_date | customer_pref_delivery_date |
+-------------+-------------+------------+-----------------------------+
| 1           | 1           | 2019-08-01 | 2019-08-02                  |
| 2           | 2           | 2019-08-02 | 2019-08-02                  |
| 3           | 1           | 2019-08-11 | 2019-08-12                  |
| 4           | 3           | 2019-08-24 | 2019-08-24                  |
| 5           | 3           | 2019-08-21 | 2019-08-22                  |
| 6           | 2           | 2019-08-11 | 2019-08-13                  |
| 7           | 4           | 2019-08-09 | 2019-08-09                  |
+-------------+-------------+------------+-----------------------------+
<strong>Đầu ra:</strong> 
+----------------------+
| immediate_percentage |
+----------------------+
| 50.00                |
+----------------------+
<strong>Giải thích:</strong> 
Khách hàng có id 1 có đơn đầu tiên với delivery id 1 và đây là đơn đặt lịch.
Khách hàng có id 2 có đơn đầu tiên với delivery id 2 và đây là đơn giao ngay.
Khách hàng có id 3 có đơn đầu tiên với delivery id 5 và đây là đơn đặt lịch.
Khách hàng có id 4 có đơn đầu tiên với delivery id 7 và đây là đơn giao ngay.
Vì vậy, một nửa số khách hàng có đơn hàng đầu tiên được giao ngay.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Subquery

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ tính đơn đầu tiên của mỗi khách hàng. Subquery lấy `MIN(order_date)` theo từng `customer_id`; query bên ngoài lọc các dòng đó rồi tính trung bình của biểu thức kiểm tra hai ngày có bằng nhau, nhân với $100$.

<!-- thinking:end -->

Ta có thể dùng subquery để tìm đơn hàng đầu tiên của mỗi khách hàng, sau đó tính tỷ lệ đơn giao ngay.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    ROUND(AVG(order_date = customer_pref_delivery_date) * 100, 2) AS immediate_percentage
FROM Delivery
WHERE
    (customer_id, order_date) IN (
        SELECT customer_id, MIN(order_date)
        FROM Delivery
        GROUP BY 1
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng subquery với `IN` trên cặp giá trị. `RANK() OVER (PARTITION BY customer_id ORDER BY order_date)` đánh dấu các đơn đầu tiên; lọc $rk=1$ rồi tính trung bình, không cần subquery.

<!-- thinking:end -->

Ta có thể dùng window function `RANK()` để xếp hạng đơn hàng của mỗi khách theo ngày đặt tăng dần, rồi lọc các đơn có hạng $1$ (đơn đầu tiên của từng khách). Sau đó, tính tỷ lệ đơn giao ngay.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY customer_id
                ORDER BY order_date
            ) AS rk
        FROM Delivery
    )
SELECT
    ROUND(AVG(order_date = customer_pref_delivery_date) * 100, 2) AS immediate_percentage
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
