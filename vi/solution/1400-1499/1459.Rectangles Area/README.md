---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1459. Rectangles Area 🔒](https://leetcode.com/problems/rectangles-area)

[Tài liệu tiếng Trung](/solution/1400-1499/1459.Rectangles%20Area/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Points</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| x_value       | int     |
| y_value       | int     |
+---------------+---------+
id là cột có các giá trị duy nhất trong bảng này.
Mỗi điểm được biểu diễn bằng một tọa độ 2D (x_value, y_value).</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để liệt kê tất cả các hình chữ nhật <strong>song song với các trục tọa độ</strong> có <strong>diện tích khác 0</strong> và có thể được tạo bởi bất kỳ hai điểm nào trong bảng <code>Points</code>.</p>

<p>Mỗi hàng trong kết quả phải chứa ba cột <code>(p1, p2, area)</code>, trong đó:</p>

<ul>
	<li><code>p1</code> và <code>p2</code> là các <code>id</code> của hai điểm xác định hai đỉnh đối diện của một hình chữ nhật.</li>
	<li><code>area</code> là diện tích của hình chữ nhật và phải <strong>khác 0</strong>.</li>
</ul>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>area</code> theo thứ tự <strong>giảm dần</strong>. Nếu bằng nhau, sắp xếp theo <code>p1</code> theo thứ tự <strong>tăng dần</strong>. Nếu vẫn bằng nhau, sắp xếp theo <code>p2</code> theo thứ tự <strong>tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong bảng sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1459.Rectangles%20Area/images/rect.png" style="width: 303px; height: 500px;" />
<pre>
<strong>Đầu vào:</strong>
Bảng Points:
+----------+-------------+-------------+
| id       | x_value     | y_value     |
+----------+-------------+-------------+
| 1        | 2           | 7           |
| 2        | 4           | 8           |
| 3        | 2           | 10          |
+----------+-------------+-------------+
<strong>Đầu ra:</strong>
+----------+-------------+-------------+
| p1       | p2          | area        |
+----------+-------------+-------------+
| 2        | 3           | 4           |
| 1        | 2           | 2           |
+----------+-------------+-------------+
<strong>Giải thích:</strong>
Hình chữ nhật được tạo bởi p1 = 2 và p2 = 3 có diện tích bằng |4-2| * |8-10| = 4.
Hình chữ nhật được tạo bởi p1 = 1 và p2 = 2 có diện tích bằng |2-4| * |7-8| = 2.
Lưu ý rằng hình chữ nhật được tạo bởi p1 = 1 và p2 = 3 không hợp lệ vì diện tích bằng 0.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bất kỳ hai điểm nào có $x$ khác nhau và $y$ khác nhau đều xác định một hình chữ nhật song song với các trục tọa độ và có diện tích dương. Thực hiện self-join với $p1.id<p2.id$, loại bỏ các cặp thẳng hàng, rồi tính $|\Delta x|\cdot|\Delta y|$ và sắp xếp theo diện tích rồi đến các id.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    p1.id AS p1,
    p2.id AS p2,
    ABS(p1.x_value - p2.x_value) * ABS(p1.y_value - p2.y_value) AS area
FROM
    Points AS p1
    JOIN Points AS p2 ON p1.id < p2.id
WHERE p1.x_value != p2.x_value AND p1.y_value != p2.y_value
ORDER BY area DESC, p1, p2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
