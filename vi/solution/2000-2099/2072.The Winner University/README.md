---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2072. The Winner University 🔒](https://leetcode.com/problems/the-winner-university)

[中文文档](/solution/2000-2099/2072.The%20Winner%20University/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>NewYork</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| student_id  | int  |
| score       | int  |
+-------------+------+
Trong SQL, student_id là khóa chính của bảng này.
Mỗi hàng chứa thông tin về điểm thi của một sinh viên đến từ New York University.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>California</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| student_id  | int  |
| score       | int  |
+-------------+------+
Trong SQL, student_id là khóa chính của bảng này.
Mỗi hàng chứa thông tin về điểm thi của một sinh viên đến từ California University.
</pre>

<p>&nbsp;</p>

<p>Có một cuộc thi giữa New York University và California University. Cuộc thi được tổ chức giữa cùng một số lượng sinh viên của hai trường. Trường có nhiều <strong>sinh viên xuất sắc</strong> hơn sẽ thắng cuộc thi. Nếu hai trường có cùng số lượng <strong>sinh viên xuất sắc</strong>, cuộc thi kết thúc với kết quả hòa.</p>

<p><strong>Sinh viên xuất sắc</strong> là sinh viên đạt <code>90%</code> điểm trở lên trong kỳ thi.</p>

<p>Trả về:</p>

<ul>
	<li><strong>&quot;New York University&quot;</strong> nếu New York University thắng cuộc thi.</li>
	<li><strong>&quot;California University&quot;</strong> nếu California University thắng cuộc thi.</li>
	<li><strong>&quot;No Winner&quot;</strong> nếu cuộc thi kết thúc với kết quả hòa.</li>
</ul>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng NewYork:
+------------+-------+
| student_id | score |
+------------+-------+
| 1          | 90    |
| 2          | 87    |
+------------+-------+
Bảng California:
+------------+-------+
| student_id | score |
+------------+-------+
| 2          | 89    |
| 3          | 88    |
+------------+-------+
<strong>Đầu ra:</strong>
+---------------------+
| winner              |
+---------------------+
| New York University |
+---------------------+
<strong>Giải thích:</strong>
New York University có 1 sinh viên xuất sắc, còn California University có 0 sinh viên xuất sắc.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng NewYork:
+------------+-------+
| student_id | score |
+------------+-------+
| 1          | 89    |
| 2          | 88    |
+------------+-------+
Bảng California:
+------------+-------+
| student_id | score |
+------------+-------+
| 2          | 90    |
| 3          | 87    |
+------------+-------+
<strong>Đầu ra:</strong>
+-----------------------+
| winner                |
+-----------------------+
| California University |
+-----------------------+
<strong>Giải thích:</strong>
New York University có 0 sinh viên xuất sắc, còn California University có 1 sinh viên xuất sắc.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng NewYork:
+------------+-------+
| student_id | score |
+------------+-------+
| 1          | 89    |
| 2          | 90    |
+------------+-------+
Bảng California:
+------------+-------+
| student_id | score |
+------------+-------+
| 2          | 87    |
| 3          | 99    |
+------------+-------+
<strong>Đầu ra:</strong>
+-----------+
| winner    |
+-----------+
| No Winner |
+-----------+
<strong>Giải thích:</strong>
Cả New York University và California University đều có 1 sinh viên xuất sắc.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> So sánh số lượng điểm số $\ge 90$ ở mỗi trường. Hai bảng có cấu trúc giống nhau: dùng `COUNT` để đếm từng bảng, sau đó dùng `CASE` để xác định count nào lớn hơn (hoặc hai count bằng nhau).
>
> Không cần join; tích Descartes gồm hai hàng của các phép tổng hợp là đủ.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    CASE
        WHEN n1.cnt > n2.cnt THEN 'New York University'
        WHEN n1.cnt < n2.cnt THEN 'California University'
        ELSE 'No Winner'
    END AS winner
FROM
    (SELECT COUNT(1) AS cnt FROM NewYork WHERE score >= 90) AS n1,
    (SELECT COUNT(1) AS cnt FROM California WHERE score >= 90) AS n2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
