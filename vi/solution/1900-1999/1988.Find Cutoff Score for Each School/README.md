---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1988. Find Cutoff Score for Each School 🔒](https://leetcode.com/problems/find-cutoff-score-for-each-school)

[中文文档](/solution/1900-1999/1988.Find%20Cutoff%20Score%20for%20Each%20School/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Schools</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| school_id   | int  |
| capacity    | int  |
+-------------+------+
school_id là cột chứa các giá trị duy nhất của bảng này.
Bảng này chứa thông tin về sức chứa của một số trường học. Sức chứa là số lượng học sinh tối đa mà trường có thể tiếp nhận.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Exam</code></p>

<pre>
+---------------+------+
| Column Name   | Type |
+---------------+------+
| score         | int  |
| student_count | int  |
+---------------+------+
score là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng trong bảng này cho biết có student_count học sinh đạt ít nhất score điểm trong kỳ thi.
Dữ liệu trong bảng này luôn hợp lệ về mặt logic, nghĩa là một hàng ghi nhận điểm cao hơn sẽ có student_count bằng hoặc nhỏ hơn so với một hàng ghi nhận điểm thấp hơn. Cụ thể hơn, với mọi hai hàng i và j trong bảng, nếu score<sub>i</sub> &gt; score<sub>j</sub> thì student_count<sub>i</sub> &lt;= student_count<sub>j</sub>.
</pre>

<p>&nbsp;</p>

<p>Mỗi năm, mỗi trường công bố một <strong>điểm tối thiểu</strong> mà học sinh cần đạt để đăng ký vào trường. Trường chọn điểm tối thiểu này dựa trên kết quả thi của tất cả học sinh:</p>

<ol>
	<li>Họ muốn đảm bảo rằng ngay cả khi <strong>tất cả</strong> học sinh đạt yêu cầu đều đăng ký, trường vẫn có thể tiếp nhận tất cả.</li>
	<li>Họ cũng muốn <strong>tối đa hóa</strong> số học sinh có thể đăng ký.</li>
	<li>Họ <strong>bắt buộc</strong> phải sử dụng một điểm có trong bảng <code>Exam</code>.</li>
</ol>

<p>Hãy viết lời giải để báo cáo <strong>điểm tối thiểu</strong> cho từng trường. Nếu có nhiều giá trị điểm thỏa mãn các điều kiện trên, hãy chọn giá trị <strong>nhỏ nhất</strong>. Nếu dữ liệu đầu vào không đủ để xác định điểm, hãy trả về <code>-1</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Schools:
+-----------+----------+
| school_id | capacity |
+-----------+----------+
| 11        | 151      |
| 5         | 48       |
| 9         | 9        |
| 10        | 99       |
+-----------+----------+
Bảng Exam:
+-------+---------------+
| score | student_count |
+-------+---------------+
| 975   | 10            |
| 966   | 60            |
| 844   | 76            |
| 749   | 76            |
| 744   | 100           |
+-------+---------------+
<strong>Đầu ra:</strong>
+-----------+-------+
| school_id | score |
+-----------+-------+
| 5         | 975   |
| 9         | -1    |
| 10        | 749   |
| 11        | 744   |
+-----------+-------+
<strong>Giải thích:</strong>
- Trường 5: Sức chứa của trường là 48. Nếu chọn 975 làm điểm tối thiểu, trường sẽ nhận được nhiều nhất 10 đơn đăng ký, vẫn nằm trong sức chứa.
- Trường 10: Nếu chọn 844 hoặc 749 làm điểm tối thiểu, trường sẽ nhận được nhiều nhất 76 đơn đăng ký, vẫn nằm trong sức chứa. Ta chọn giá trị nhỏ hơn là 749.
- Trường 11: Nếu chọn 744 làm điểm tối thiểu, trường sẽ nhận được nhiều nhất 100 đơn đăng ký, vẫn nằm trong sức chứa.
- Trường 9: Dữ liệu đã cho không đủ để xác định điểm tối thiểu. Nếu chọn 975 làm điểm tối thiểu, trường có thể nhận 10 đơn đăng ký trong khi sức chứa chỉ là 9. Ta không có thông tin về các điểm cao hơn, nên trả về -1.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi trường cần điểm thi thấp nhất có số học sinh phù hợp với sức chứa, hoặc $-1$. Dùng left join với $\texttt{Exam}$ để giữ lại các trường không có điểm phù hợp, sau đó dùng $\texttt{MIN}$ kết hợp với $\texttt{IFNULL}(\cdot,-1)$ cho từng trường.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT school_id, MIN(IFNULL(score, -1)) AS score
FROM
    Schools AS s
    LEFT JOIN Exam AS e ON s.capacity >= e.student_count
GROUP BY school_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
