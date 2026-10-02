---
comments: true
difficulty: Easy
tags:
    - Database
    - Closest Pair of Points
---

<!-- problem:start -->

# [613. Shortest Distance in a Line 🔒](https://leetcode.com/problems/shortest-distance-in-a-line)

[中文文档](/solution/0600-0699/0613.Shortest%20Distance%20in%20a%20Line/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Point</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
+-------------+------+
Trong SQL, x là cột khóa chính của bảng này.
Mỗi hàng trong bảng cho biết vị trí của một điểm trên trục X.
</pre>

<p>&nbsp;</p>

<p>Tìm khoảng cách ngắn nhất giữa hai điểm bất kỳ trong bảng <code>Point</code>.</p>

<p>Đảm bảo bảng <code>Point</code> có <strong>ít nhất </strong>hai hàng.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Point:
+----+
| x  |
+----+
| -1 |
| 0  |
| 2  |
+----+
<strong>Đầu ra:</strong> 
+----------+
| shortest |
+----------+
| 1        |
+----------+
<strong>Giải thích:</strong> Khoảng cách ngắn nhất là giữa hai điểm -1 và 0, bằng |(-1) - 0| = 1.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể tối ưu lời giải như thế nào nếu bảng <code>Point</code> được sắp xếp theo <strong>thứ tự tăng dần</strong>?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Self-Join

<!-- thinking:start -->

> **Tư duy**
>
> Trên một đường thẳng, khoảng cách ngắn nhất là hiệu dương nhỏ nhất giữa hai điểm. Chỉ cần self-join với điều kiện `p1.x < p2.x` rồi lấy `MIN(p2.x - p1.x)`.

<!-- thinking:end -->

Ta có thể self-join mỗi điểm trong bảng với các điểm lớn hơn nó, rồi tính khoảng cách giữa hai điểm. Cuối cùng, lấy khoảng cách nhỏ nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT MIN(p2.x - p1.x) AS shortest
FROM
    Point AS p1
    JOIN Point AS p2 ON p1.x < p2.x;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách nhỏ nhất phải nằm giữa hai điểm liền kề sau khi sắp xếp, nên không cần join toàn bộ cặp. `LAG(x)` lấy điểm đứng trước; hiệu dương nhỏ nhất chính là đáp án.

<!-- thinking:end -->

Ta có thể dùng window function để sắp xếp các điểm trong bảng theo giá trị $x$, sau đó tính khoảng cách giữa các điểm liền kề. Cuối cùng, lấy khoảng cách nhỏ nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT x - LAG(x) OVER (ORDER BY x) AS shortest
FROM Point
ORDER BY 1
LIMIT 1, 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
