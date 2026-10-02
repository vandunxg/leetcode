---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1322. Ads Performance 🔒](https://leetcode.com/problems/ads-performance)

[中文文档](/solution/1300-1399/1322.Ads%20Performance/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Ads</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| ad_id         | int     |
| user_id       | int     |
| action        | enum    |
+---------------+---------+
(ad_id, user_id) là khóa chính của bảng này (kết hợp các cột có giá trị duy nhất).
Mỗi hàng của bảng này chứa ID của một quảng cáo, ID của người dùng và hành động người dùng đó thực hiện với quảng cáo.
Cột action có kiểu ENUM (danh mục), gồm các giá trị (&#39;Clicked&#39;, &#39;Viewed&#39;, &#39;Ignored&#39;).
</pre>

<p>&nbsp;</p>

<p>Một công ty đang chạy quảng cáo và muốn tính hiệu quả của từng quảng cáo.</p>

<p>Hiệu quả của quảng cáo được đo bằng tỷ lệ nhấp (Click-Through Rate, CTR), được tính như sau:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1322.Ads%20Performance/images/sql1.png" style="width: 600px; height: 54px;" />
<p>Hãy viết lời giải để tìm <code>ctr</code> của từng quảng cáo. <strong>Làm tròn</strong> <code>ctr</code> đến <strong>hai chữ số thập phân</strong>.</p>

<p>Trả về bảng kết quả, sắp xếp theo <code>ctr</code> giảm dần; nếu bằng nhau thì sắp xếp theo <code>ad_id</code> tăng dần.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Ads:
+-------+---------+---------+
| ad_id | user_id | action  |
+-------+---------+---------+
| 1     | 1       | Clicked |
| 2     | 2       | Clicked |
| 3     | 3       | Viewed  |
| 5     | 5       | Ignored |
| 1     | 7       | Ignored |
| 2     | 7       | Viewed  |
| 3     | 5       | Clicked |
| 1     | 4       | Viewed  |
| 2     | 11      | Viewed  |
| 1     | 2       | Clicked |
+-------+---------+---------+
<strong>Đầu ra:</strong> 
+-------+-------+
| ad_id | ctr   |
+-------+-------+
| 1     | 66.67 |
| 3     | 50.00 |
| 2     | 33.33 |
| 5     | 0.00  |
+-------+-------+
<strong>Giải thích:</strong> 
với ad_id = 1, ctr = (2/(2+1)) * 100 = 66.67
với ad_id = 2, ctr = (1/(1+2)) * 100 = 33.33
với ad_id = 3, ctr = (1/(1+1)) * 100 = 50.00
với ad_id = 5, ctr = 0.00. Lưu ý rằng ad_id = 5 không có lượt nhấp hay lượt xem.
Không cần xét các quảng cáo bị bỏ qua (Ignored).
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> CTR bằng số lượt nhấp chia cho tổng lượt nhấp và lượt xem; bỏ qua $\textit{Ignored}$, và nếu không có dữ liệu để tính tỷ lệ thì kết quả là $0$. Group theo $\textit{ad\_id}$ cùng các phép tính tổng có điều kiện sẽ cho ta cả hai số lượng; $\mathrm{IFNULL}$ xử lý trường hợp chia cho 0, sau đó sắp xếp CTR giảm dần và ID tăng dần.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    ad_id,
    ROUND(IFNULL(SUM(action = 'Clicked') / SUM(action IN ('Clicked', 'Viewed')) * 100, 0), 2) AS ctr
FROM Ads
GROUP BY 1
ORDER BY 2 DESC, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
