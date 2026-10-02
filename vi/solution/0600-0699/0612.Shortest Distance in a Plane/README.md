---
comments: true
difficulty: Medium
tags:
    - Database
    - Closest Pair of Points
---

<!-- problem:start -->

# [612. Shortest Distance in a Plane 🔒](https://leetcode.com/problems/shortest-distance-in-a-plane)

[中文文档](/solution/0600-0699/0612.Shortest%20Distance%20in%20a%20Plane/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Point2D</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
| y           | int  |
+-------------+------+
(x, y) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết vị trí của một điểm trên mặt phẳng X-Y.
</pre>

<p>&nbsp;</p>

<p>Khoảng cách giữa hai điểm <code>p<sub>1</sub>(x<sub>1</sub>, y<sub>1</sub>)</code> và <code>p<sub>2</sub>(x<sub>2</sub>, y<sub>2</sub>)</code> được tính bằng <code>sqrt((x<sub>2</sub> - x<sub>1</sub>)<sup>2</sup> + (y<sub>2</sub> - y<sub>1</sub>)<sup>2</sup>)</code>.</p>

<p>Hãy viết lời giải để tìm khoảng cách ngắn nhất giữa hai điểm bất kỳ trong bảng <code>Point2D</code>. Làm tròn khoảng cách đến <strong>hai chữ số thập phân</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Point2D:
+----+----+
| x  | y  |
+----+----+
| -1 | -1 |
| 0  | 0  |
| -1 | -2 |
+----+----+
<strong>Đầu ra:</strong> 
+----------+
| shortest |
+----------+
| 1.00     |
+----------+
<strong>Giải thích:</strong> Khoảng cách ngắn nhất là 1.00, từ điểm (-1, -1) đến (-1, 2).
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách ngắn nhất trên mặt phẳng là khoảng cách Euclid nhỏ nhất giữa các cặp điểm khác nhau. Bảng đủ nhỏ để dùng self-join.
>
> Loại các điểm trùng nhau, tính `SQRT` của tổng bình phương các hiệu tọa độ, dùng `ROUND` để làm tròn đến hai chữ số thập phân rồi lấy giá trị nhỏ nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT ROUND(SQRT(POW(p1.x - p2.x, 2) + POW(p1.y - p2.y, 2)), 2) AS shortest
FROM
    Point2D AS p1
    JOIN Point2D AS p2 ON p1.x != p2.x OR p1.y != p2.y
ORDER BY 1
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
