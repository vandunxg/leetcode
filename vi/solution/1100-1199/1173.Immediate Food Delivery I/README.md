---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1173. Immediate Food Delivery I 🔒](https://leetcode.com/problems/immediate-food-delivery-i)

[中文文档](/solution/1100-1199/1173.Immediate%20Food%20Delivery%20I/README.md)

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
delivery_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Bảng lưu thông tin giao đồ ăn cho khách hàng đặt hàng vào một ngày nào đó và chọn ngày giao mong muốn (cùng ngày đặt hàng hoặc sau đó).
</pre>

<p>&nbsp;</p>

<p>Nếu ngày giao mong muốn của khách hàng trùng với ngày đặt hàng thì đơn hàng được gọi là <strong>giao ngay;</strong> nếu không thì đó là đơn hàng <strong>được lên lịch</strong>.</p>

<p>Hãy viết lời giải để tìm tỷ lệ phần trăm đơn hàng giao ngay trong bảng, <strong>làm tròn đến 2 chữ số thập phân</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Delivery:
+-------------+-------------+------------+-----------------------------+
| delivery_id | customer_id | order_date | customer_pref_delivery_date |
+-------------+-------------+------------+-----------------------------+
| 1           | 1           | 2019-08-01 | 2019-08-02                  |
| 2           | 5           | 2019-08-02 | 2019-08-02                  |
| 3           | 1           | 2019-08-11 | 2019-08-11                  |
| 4           | 3           | 2019-08-24 | 2019-08-26                  |
| 5           | 4           | 2019-08-21 | 2019-08-22                  |
| 6           | 2           | 2019-08-11 | 2019-08-13                  |
+-------------+-------------+------------+-----------------------------+
<strong>Đầu ra:</strong> 
+----------------------+
| immediate_percentage |
+----------------------+
| 33.33                |
+----------------------+
<strong>Giải thích:</strong> Các đơn hàng có delivery_id bằng 2 và 3 là đơn giao ngay, còn lại là đơn được lên lịch.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1: Sum

<!-- thinking:start -->

> **Tư duy**
>
> Đơn hàng giao ngay thỏa mãn `order_date = customer_pref_delivery_date`. Biểu thức boolean này khi tính tổng sẽ cho giá trị $0/1$; chia cho số hàng, nhân với $100$ rồi làm tròn. Không cần lọc riêng rồi mới đếm.

<!-- thinking:end -->

Ta có thể dùng hàm `sum` để đếm số đơn hàng giao ngay, sau đó chia cho tổng số đơn hàng. Vì đề bài yêu cầu tỷ lệ phần trăm, ta nhân kết quả với 100. Cuối cùng, dùng hàm `round` để giữ lại hai chữ số thập phân.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    ROUND(SUM(order_date = customer_pref_delivery_date) / COUNT(1) * 100, 2) AS immediate_percentage
FROM Delivery;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
