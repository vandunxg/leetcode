---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2978. Symmetric Coordinates 🔒](https://leetcode.com/problems/symmetric-coordinates)

[中文文档](/solution/2900-2999/2978.Symmetric%20Coordinates/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace"><code>Coordinates</code></font></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| X           | int  |
| Y           | int  |
+-------------+------+
Mỗi hàng gồm X và Y, trong đó cả hai đều là số nguyên. Bảng có thể chứa các giá trị trùng lặp.
</pre>

<p>Hai tọa độ <code>(X1, Y1)</code> và <code>(X2, Y2)</code> được gọi là <strong>đối xứng</strong> nếu <code>X1 == Y2</code> và <code>X2 == Y1</code>.</p>

<p>Hãy viết lời giải để xuất, trong số tất cả các tọa độ <strong>đối xứng</strong> <strong>này</strong>, chỉ những tọa độ <strong>duy nhất</strong> thỏa mãn điều kiện <code>X1 &lt;= Y1</code>.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>X</code> <em>và </em> <code>Y</code> <em>(tương ứng)</em> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Coordinates:
+----+----+
| X  | Y  |
+----+----+
| 20 | 20 |
| 20 | 20 |
| 20 | 21 |
| 23 | 22 |
| 22 | 23 |
| 21 | 20 |
+----+----+
<strong>Đầu ra:</strong>
+----+----+
| x  | y  |
+----+----+
| 20 | 20 |
| 20 | 21 |
| 22 | 23 |
+----+----+
<strong>Giải thích:</strong>
- (20, 20) và (20, 20) là các tọa độ đối xứng vì X1 == Y2 và X2 == Y1. Do đó, chỉ hiển thị (20, 20) như một tọa độ duy nhất.
- (20, 21) và (21, 20) là các tọa độ đối xứng vì X1 == Y2 và X2 == Y1. Tuy nhiên, chỉ hiển thị (20, 21) vì X1 &lt;= Y1.
- (23, 22) và (22, 23) là các tọa độ đối xứng vì X1 == Y2 và X2 == Y1. Tuy nhiên, chỉ hiển thị (22, 23) vì X1 &lt;= Y1.
Bảng kết quả được sắp xếp theo X và Y theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ + Phép tự nối

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp đối xứng là $(x,y)$ cùng với $(y,x)$ từ các hàng khác nhau. Các tọa độ trùng lặp cần có id hàng riêng. $ROW_NUMBER$ cung cấp $id$; phép tự nối yêu cầu $p1.x=p2.y$, $p1.y=p2.x$, $p1.x \le p1.y$ và id khác nhau để một điểm không tự ghép với chính nó, đồng thời mỗi cặp chỉ được liệt kê một lần.
>
> Kết quả là các tọa độ phân biệt được sắp xếp.

<!-- thinking:end -->

Ta có thể dùng hàm cửa sổ `ROW_NUMBER()` để thêm một số thứ tự tự tăng vào mỗi hàng. Sau đó, ta thực hiện phép tự nối trên hai bảng, với các điều kiện nối là `p1.x = p2.y AND p1.y = p2.x AND p1.x <= p1.y AND p1.id != p2.id`. Cuối cùng, ta sắp xếp và loại bỏ các bản ghi trùng lặp.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT
            ROW_NUMBER() OVER () AS id,
            x,
            y
        FROM Coordinates
    )
SELECT DISTINCT
    p1.x,
    p1.y
FROM
    P AS p1
    JOIN P AS p2 ON p1.x = p2.y AND p1.y = p2.x AND p1.x <= p1.y AND p1.id != p2.id
ORDER BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
