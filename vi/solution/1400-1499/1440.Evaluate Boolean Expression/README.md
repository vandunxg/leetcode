---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1440. Evaluate Boolean Expression 🔒](https://leetcode.com/problems/evaluate-boolean-expression)

[中文文档](/solution/1400-1499/1440.Evaluate%20Boolean%20Expression/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng <code>Variables</code>:</p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| name          | varchar |
| value         | int     |
+---------------+---------+
Trong SQL, name là khóa chính của bảng này.
Bảng này chứa các biến được lưu trữ và giá trị của chúng.
</pre>

<p>&nbsp;</p>

<p>Bảng <code>Expressions</code>:</p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| left_operand  | varchar |
| operator      | enum    |
| right_operand | varchar |
+---------------+---------+
Trong SQL, (left_operand, operator, right_operand) là khóa chính của bảng này.
Bảng này chứa một biểu thức boolean cần được đánh giá.
operator là một enum nhận một trong các giá trị (&#39;&lt;&#39;, &#39;&gt;&#39;, &#39;=&#39;)
Các giá trị của left_operand và right_operand được đảm bảo tồn tại trong bảng Variables.
</pre>

<p>&nbsp;</p>

<p>Đánh giá các biểu thức boolean trong bảng <code>Expressions</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Variables:
+------+-------+
| name | value |
+------+-------+
| x    | 66    |
| y    | 77    |
+------+-------+
Bảng Expressions:
+--------------+----------+---------------+
| left_operand | operator | right_operand |
+--------------+----------+---------------+
| x            | &gt;        | y             |
| x            | &lt;        | y             |
| x            | =        | y             |
| y            | &gt;        | x             |
| y            | &lt;        | x             |
| x            | =        | x             |
+--------------+----------+---------------+
<strong>Đầu ra:</strong>
+--------------+----------+---------------+-------+
| left_operand | operator | right_operand | value |
+--------------+----------+---------------+-------+
| x            | &gt;        | y             | false |
| x            | &lt;        | y             | true  |
| x            | =        | y             | false |
| y            | &gt;        | x             | true  |
| y            | &lt;        | x             | false |
| x            | =        | x             | true  |
+--------------+----------+---------------+-------+
<strong>Giải thích:</strong>
Như minh họa, cần tìm giá trị của từng biểu thức boolean trong bảng bằng cách sử dụng bảng biến.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + biểu thức CASE

<!-- thinking:start -->

> **Tư duy**
>
> `Expressions` lưu tên, còn giá trị nằm trong `Variables`. Join `Variables` hai lần để liên kết các toán hạng trái và phải.
>
> `CASE` đánh giá `=`, `>`, và `<`, rồi trả về `true` hoặc `false`.

<!-- thinking:end -->

Ta có thể liên kết mỗi hàng trong bảng `Expressions` với hai hàng trong bảng `Variables` bằng equi-join, trong đó các điều kiện liên kết là `left_operand = name` và `right_operand = name`. Sau đó, ta có thể sử dụng biểu thức `CASE` để xác định giá trị của biểu thức boolean. Nếu `operator` là `=`, ta kiểm tra xem hai giá trị có bằng nhau hay không. Nếu `operator` là `>`, ta kiểm tra xem giá trị bên trái có lớn hơn giá trị bên phải hay không. Nếu `operator` là `<`, ta kiểm tra xem giá trị bên trái có nhỏ hơn giá trị bên phải hay không. Nếu điều kiện đúng, biểu thức boolean nhận giá trị `true`, ngược lại nhận giá trị `false`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    left_operand,
    operator,
    right_operand,
    CASE
        WHEN (
            (operator = '=' AND v1.value = v2.value)
            OR (operator = '>' AND v1.value > v2.value)
            OR (operator = '<' AND v1.value < v2.value)
        ) THEN 'true'
        ELSE 'false'
    END AS value
FROM
    Expressions AS e
    JOIN Variables AS v1 ON e.left_operand = v1.name
    JOIN Variables AS v2 ON e.right_operand = v2.name;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
