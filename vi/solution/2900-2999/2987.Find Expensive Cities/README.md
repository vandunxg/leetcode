---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2987. Find Expensive Cities 🔒](https://leetcode.com/problems/find-expensive-cities)

[Tài liệu tiếng Trung](/solution/2900-2999/2987.Find%20Expensive%20Cities/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Listings</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| listing_id  | int     |
| city        | varchar |
| price       | int     |
+-------------+---------+
listing_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa listing_id, city và price.
</pre>

<p>Hãy viết lời giải để tìm các <strong>thành phố</strong> có <strong>giá nhà trung bình</strong> cao hơn <strong>giá nhà trung bình trên toàn quốc</strong>.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>city</code><em> theo thứ tự <strong>tăng dần</strong></em><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Listings table:
+------------+--------------+---------+
| listing_id | city         | price   |
+------------+--------------+---------+
| 113        | LosAngeles   | 7560386 |
| 136        | SanFrancisco | 2380268 |
| 92         | Chicago      | 9833209 |
| 60         | Chicago      | 5147582 |
| 8          | Chicago      | 5274441 |
| 79         | SanFrancisco | 8372065 |
| 37         | Chicago      | 7939595 |
| 53         | LosAngeles   | 4965123 |
| 178        | SanFrancisco | 999207  |
| 51         | NewYork      | 5951718 |
| 121        | NewYork      | 2893760 |
+------------+--------------+---------+
<strong>Đầu ra</strong>
+------------+
| city       |
+------------+
| Chicago    |
| LosAngeles |
+------------+
<strong>Giải thích</strong>
Giá nhà trung bình trên toàn quốc là $6,122,059.45. Trong các thành phố được liệt kê:
- Chicago có giá trung bình là $7,048,706.75
- Los Angeles có giá trung bình là $6,277,754.5
- San Francisco có giá trung bình là $3,900,513.33
- New York có giá trung bình là $4,422,739
Chỉ Chicago và Los Angeles có giá nhà trung bình cao hơn mức trung bình trên toàn quốc. Vì vậy, hai thành phố này được đưa vào bảng kết quả. Bảng kết quả được sắp xếp theo thứ tự tăng dần dựa trên tên thành phố.

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng hợp theo nhóm + Truy vấn con

<!-- thinking:start -->

> **Tư duy**
>
> Một thành phố đủ điều kiện khi giá nhà trung bình của thành phố đó cao hơn mức trung bình trên toàn quốc. Nhóm theo $city$ và dùng $HAVING AVG(price)$ để so sánh với truy vấn con vô hướng $AVG(price)$ được tính trên toàn bộ bảng.
>
> Sắp xếp theo tên thành phố.

<!-- thinking:end -->

Ta nhóm bảng `Listings` theo `city`, sau đó tính giá nhà trung bình cho từng thành phố và cuối cùng lọc các thành phố có giá nhà trung bình cao hơn giá nhà trung bình trên toàn quốc.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT city
FROM Listings
GROUP BY city
HAVING AVG(price) > (SELECT AVG(price) FROM Listings)
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
