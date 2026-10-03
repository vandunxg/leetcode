---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2026. Low-Quality Problems 🔒](https://leetcode.com/problems/low-quality-problems)

[中文文档](/solution/2000-2099/2026.Low-Quality%20Problems/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Problems</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| problem_id  | int  |
| likes       | int  |
| dislikes    | int  |
+-------------+------+
Trong SQL, problem_id là cột khóa chính của bảng này.
Mỗi hàng của bảng này cho biết số lượt thích và không thích đối với một bài toán LeetCode.
</pre>

<p>&nbsp;</p>

<p>Hãy tìm ID của các bài toán <strong>chất lượng thấp</strong>. Một bài toán LeetCode được xem là <strong>chất lượng thấp</strong> nếu tỷ lệ lượt thích của bài toán (số lượt thích chia cho tổng số lượt bình chọn) <strong>nhỏ hơn</strong> <code>60%</code>.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>problem_id</code> tăng dần.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Problems:
+------------+-------+----------+
| problem_id | likes | dislikes |
+------------+-------+----------+
| 6          | 1290  | 425      |
| 11         | 2677  | 8659     |
| 1          | 4446  | 2760     |
| 7          | 8569  | 6086     |
| 13         | 2050  | 4164     |
| 10         | 9002  | 7446     |
+------------+-------+----------+
<strong>Đầu ra:</strong>
+------------+
| problem_id |
+------------+
| 7          |
| 10         |
| 11         |
| 13         |
+------------+
<strong>Giải thích:</strong> Tỷ lệ lượt thích như sau:
- Bài toán 1: (4446 / (4446 + 2760)) * 100 = 61.69858%
- Bài toán 6: (1290 / (1290 + 425)) * 100 = 75.21866%
- Bài toán 7: (8569 / (8569 + 6086)) * 100 = 58.47151%
- Bài toán 10: (9002 / (9002 + 7446)) * 100 = 54.73006%
- Bài toán 11: (2677 / (2677 + 8659)) * 100 = 23.61503%
- Bài toán 13: (2050 / (2050 + 4164)) * 100 = 32.99002%
Các bài toán 7, 10, 11 và 13 là các bài toán chất lượng thấp vì tỷ lệ lượt thích của chúng nhỏ hơn 60%.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chất lượng được tính bằng $likes/(likes+dislikes)$; ta cần các ID có tỷ lệ nhỏ hơn $0.6$, được sắp xếp. Không cần phép join hay grouping.
>
> Chỉ cần một `WHERE` để biểu diễn tỷ lệ và `ORDER BY problem_id` để cố định thứ tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT problem_id
FROM Problems
WHERE likes / (likes + dislikes) < 0.6
ORDER BY problem_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
