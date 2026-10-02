---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [610. Triangle Judgement](https://leetcode.com/problems/triangle-judgement)

[中文文档](/solution/0600-0699/0610.Triangle%20Judgement/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Triangle</code></p>

<pre>
+-------------+------+
| Tên cột    | Kiểu |
+-------------+------+
| x           | int  |
| y           | int  |
| z           | int  |
+-------------+------+
Trong SQL, (x, y, z) là khóa chính của bảng này.
Mỗi hàng trong bảng chứa độ dài của ba đoạn thẳng.
</pre>

<p>&nbsp;</p>

<p>Với mỗi bộ ba đoạn thẳng, hãy cho biết chúng có thể tạo thành một tam giác hay không.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Triangle:
+----+----+----+
| x  | y  | z  |
+----+----+----+
| 13 | 15 | 30 |
| 10 | 20 | 15 |
+----+----+----+
<strong>Đầu ra:</strong> 
+----+----+----+----------+
| x  | y  | z  | triangle |
+----+----+----+----------+
| 13 | 15 | 30 | No       |
| 10 | 20 | 15 | Yes      |
+----+----+----+----------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: IF + Bất đẳng thức tam giác

<!-- thinking:start -->

> **Tư duy**
>
> Ba độ dài tạo thành một tam giác khi và chỉ khi thỏa mãn cả ba bất đẳng thức tam giác.
>
> Dùng `IF` để kiểm tra $x+y>z$, $x+z>y$ và $y+z>x$, rồi gán cho mỗi hàng `Yes` hoặc `No`.

<!-- thinking:end -->

Điều kiện để ba cạnh tạo thành tam giác là tổng độ dài của hai cạnh bất kỳ phải lớn hơn cạnh còn lại. Vì vậy, ta có thể dùng câu lệnh `IF` để kiểm tra điều kiện này. Nếu thỏa mãn, trả về `Yes`; nếu không, trả về `No`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    *,
    IF(x + y > z AND x + z > y AND y + z > x, 'Yes', 'No') AS triangle
FROM Triangle;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
