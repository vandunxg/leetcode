---
comments: true
difficulty: Easy
tags:
    - Database
    - Polygon
---

<!-- problem:start -->

# [3053. Classifying Triangles by Lengths 🔒](https://leetcode.com/problems/classifying-triangles-by-lengths)

[中文文档](/solution/3000-3099/3053.Classifying%20Triangles%20by%20Lengths/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace"><code>Triangles</code></font></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| A           | int  |
| B           | int  |
| C           | int  |
+-------------+------+
(A, B, C) là khóa chính của bảng này.
Mỗi hàng chứa độ dài ba cạnh của một tam giác.
</pre>

<p>Viết một truy vấn để tìm loại <strong>tam giác</strong>. Với mỗi hàng, trả về một trong các kết quả sau:</p>

<ul>
	<li><strong>Equilateral</strong>: Tam giác có <code>3</code> cạnh bằng nhau.</li>
	<li><strong>Isosceles</strong>: Tam giác có <code>2</code> cạnh bằng nhau.</li>
	<li><strong>Scalene</strong>: Tam giác có <code>3</code> cạnh khác nhau.</li>
	<li><strong>Not A Triangle: </strong>Các giá trị <code>A</code>, <code>B</code> và <code>C</code> đã cho không tạo thành một tam giác.</li>
</ul>

<p><em>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Triangles:
+----+----+----+
| A  | B  | C  |
+----+----+----+
| 20 | 20 | 23 |
| 20 | 20 | 20 |
| 20 | 21 | 22 |
| 13 | 14 | 30 |
+----+----+----+
<strong>Đầu ra:</strong>
+----------------+
| triangle_type  |
+----------------+
| Isosceles      |
| Equilateral    |
| Scalene        |
| Not A Triangle |
+----------------+
<strong>Giải thích:</strong>
- Các giá trị ở hàng đầu tiên tạo thành một tam giác Isosceles vì A = B.
- Các giá trị ở hàng thứ hai tạo thành một tam giác Equilateral vì A = B = C.
- Các giá trị ở hàng thứ ba tạo thành một tam giác Scalene vì A != B != C.
- Các giá trị ở hàng thứ tư không thể tạo thành một tam giác vì tổng độ dài các cạnh A và B không lớn hơn độ dài cạnh C.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng câu lệnh CASE WHEN

<!-- thinking:start -->

> **Tư duy**
>
> Ba cạnh nằm trên cùng một hàng. Chúng ta phân biệt trường hợp không thể tạo thành tam giác, tam giác đều, tam giác cân và tam giác thường bằng một $\texttt{CASE}$ duy nhất theo thứ tự ưu tiên.
>
> Trước tiên, loại bỏ các trường hợp vi phạm bất đẳng thức tam giác, sau đó kiểm tra ba cạnh bằng nhau, rồi kiểm tra đúng một cặp cạnh bằng nhau.
>
> Tổng $(A=B)+(B=C)+(A=C)=1$ xác định tam giác cân mà không nhầm với tam giác đều.

<!-- thinking:end -->

Chúng ta có thể dùng câu lệnh `CASE WHEN` để xác định loại tam giác.

Trước tiên, cần xác định xem ba cạnh có thể tạo thành một tam giác hay không. Nếu không, trả về `Not A Triangle`.

Sau đó, kiểm tra xem độ dài của ba cạnh có bằng nhau hay không. Nếu có, trả về `Equilateral`.

Tiếp theo, kiểm tra xem có hai cạnh bằng nhau hay không. Nếu có, trả về `Isosceles`.

Nếu không thuộc các trường hợp trên, nghĩa là độ dài ba cạnh đều khác nhau, nên trả về `Scalene`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    CASE
        WHEN A + B <= C
        OR A + C <= B
        OR B + C <= A THEN 'Not A Triangle'
        WHEN A = B
        AND B = c THEN 'Equilateral'
        WHEN (A = B) + (B = C) + (A = C) = 1 THEN 'Isosceles'
        ELSE 'Scalene'
    END AS triangle_type
FROM Triangles;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
