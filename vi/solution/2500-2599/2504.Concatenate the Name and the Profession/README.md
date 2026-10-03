---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2504. Concatenate the Name and the Profession 🔒](https://leetcode.com/problems/concatenate-the-name-and-the-profession)

[中文文档](/solution/2500-2599/2504.Concatenate%20the%20Name%20and%20the%20Profession/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Person</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| person_id   | int     |
| name        | varchar |
| profession  | ENUM    |
+-------------+---------+
person_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng chứa ID, tên và nghề nghiệp của một người.
Cột profession là một enum thuộc kiểu (&#39;Doctor&#39;, &#39;Singer&#39;, &#39;Actor&#39;, &#39;Player&#39;, &#39;Engineer&#39; hoặc &#39;Lawyer&#39;)
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo tên của mỗi người, theo sau là chữ cái đầu tiên trong nghề nghiệp của họ và được đặt trong dấu ngoặc đơn.</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>person_id</code> theo <strong>thứ tự giảm dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Person:
+-----------+-------+------------+
| person_id | name  | profession |
+-----------+-------+------------+
| 1         | Alex  | Singer     |
| 3         | Alice | Actor      |
| 2         | Bob   | Player     |
| 4         | Messi | Doctor     |
| 6         | Tyson | Engineer   |
| 5         | Meir  | Lawyer     |
+-----------+-------+------------+
<strong>Đầu ra:</strong>
+-----------+----------+
| person_id | name     |
+-----------+----------+
| 6         | Tyson(E) |
| 5         | Meir(L)  |
| 4         | Messi(D) |
| 3         | Alice(A) |
| 2         | Bob(P)   |
| 1         | Alex(S)  |
+-----------+----------+
<strong>Giải thích:</strong> Lưu ý rằng không được có khoảng trắng giữa tên và chữ cái đầu tiên trong nghề nghiệp.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng cần hiển thị tên, theo sau là chữ cái đầu tiên trong nghề nghiệp được đặt trong dấu ngoặc đơn, và được sắp xếp theo $\textit{person\_id}$ giảm dần. Có thể nối chuỗi trong application code, nhưng chỉ cần một query là đủ.
>
> $\operatorname{CONCAT}$ nối tên, dấu ngoặc đơn và $\operatorname{SUBSTRING}(\textit{profession},1,1)$; sau đó sắp xếp theo $\textit{person\_id}$ giảm dần.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT person_id, CONCAT(name, "(", SUBSTRING(profession, 1, 1), ")") AS name
FROM Person
ORDER BY person_id DESC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
