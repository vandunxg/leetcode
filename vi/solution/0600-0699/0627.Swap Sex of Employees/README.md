---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [627. Swap Sex of Employees](https://leetcode.com/problems/swap-sex-of-employees)

[中文文档](/solution/0600-0699/0627.Swap%20Sex%20of%20Employees/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Salary</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| name        | varchar  |
| sex         | ENUM     |
| salary      | int      |
+-------------+----------+
id là khóa chính (cột có giá trị duy nhất) của bảng này.
Cột sex là ENUM (danh mục) với các giá trị thuộc kiểu (&#39;m&#39;, &#39;f&#39;).
Bảng chứa thông tin về nhân viên.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để đổi tất cả giá trị <code>&#39;f&#39;</code> và <code>&#39;m&#39;</code> cho nhau (tức là đổi mọi giá trị <code>&#39;f&#39;</code> thành <code>&#39;m&#39;</code> và ngược lại) bằng <strong>một câu lệnh update duy nhất</strong>, không dùng bảng tạm trung gian.</p>

<p>Lưu ý, bạn phải viết một câu lệnh update duy nhất; <strong>không</strong> được viết câu lệnh select cho bài này.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Salary:
+----+------+-----+--------+
| id | name | sex | salary |
+----+------+-----+--------+
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |
+----+------+-----+--------+
<strong>Đầu ra:</strong> 
+----+------+-----+--------+
| id | name | sex | salary |
+----+------+-----+--------+
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |
+----+------+-----+--------+
<strong>Giải thích:</strong> 
(1, A) và (3, C) được đổi từ &#39;m&#39; thành &#39;f&#39;.
(2, B) và (4, D) được đổi từ &#39;f&#39; thành &#39;m&#39;.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đổi giới tính bằng một câu lệnh UPDATE

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giá trị `'f'`/`'m'` phải được đổi sang giá trị còn lại, và đề bài yêu cầu chỉ dùng một câu lệnh `UPDATE`.
>
> `SET sex = IF(sex='f','m','f')` cập nhật trực tiếp từng hàng.

<!-- thinking:end -->

Theo yêu cầu đề bài, ta chỉ cần dùng một câu lệnh UPDATE để đổi giới tính của tất cả nhân viên. Có thể thực hiện việc này bằng biểu thức điều kiện trong SQL.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
UPDATE Salary
SET sex = IF(sex = 'f', 'm', 'f');
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
