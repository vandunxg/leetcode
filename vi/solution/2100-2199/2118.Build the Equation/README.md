---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2118. Build the Equation 🔒](https://leetcode.com/problems/build-the-equation)

[中文文档](/solution/2100-2199/2118.Build%20the%20Equation/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Terms</code></p>

<pre>
+-------------+------+
| Tên cột     | Kiểu |
+-------------+------+
| power       | int  |
| factor      | int  |
+-------------+------+
power là cột có các giá trị duy nhất trong bảng này.
Mỗi dòng trong bảng chứa thông tin về một hạng tử của phương trình.
power là một số nguyên trong khoảng [0, 100].
factor là một số nguyên trong khoảng [-100, 100] và không thể bằng 0.
</pre>

<p>&nbsp;</p>

<p>Bạn có một chương trình rất mạnh, có thể giải mọi phương trình một ẩn trên thế giới. Phương trình truyền vào chương trình phải có định dạng như sau:</p>

<ul>
	<li>Vế trái (LHS) phải chứa tất cả các hạng tử.</li>
	<li>Vế phải (RHS) phải bằng không.</li>
	<li>Mỗi hạng tử ở LHS phải có định dạng <code>&quot;&lt;sign&gt;&lt;fact&gt;X^&lt;pow&gt;&quot;</code>&nbsp;trong đó:
	<ul>
		<li><code>&lt;sign&gt;</code> là <code>&quot;+&quot;</code> hoặc <code>&quot;-&quot;</code>.</li>
		<li><code>&lt;fact&gt;</code> là <strong>giá trị tuyệt đối</strong> của <code>factor</code>.</li>
		<li><code>&lt;pow&gt;</code> là giá trị của <code>power</code>.</li>
	</ul>
	</li>
	<li>Nếu power là <code>1</code>, không thêm <code>&quot;^&lt;pow&gt;&quot;</code>.
	<ul>
		<li>Ví dụ, nếu <code>power = 1</code> và <code>factor = 3</code>, hạng tử sẽ là <code>&quot;+3X&quot;</code>.</li>
	</ul>
	</li>
	<li>Nếu power là <code>0</code>, không thêm <code>&quot;X&quot;</code> cũng như <code>&quot;^&lt;pow&gt;&quot;</code>.
	<ul>
		<li>Ví dụ, nếu <code>power = 0</code> và <code>factor = -3</code>, hạng tử sẽ là <code>&quot;-3&quot;</code>.</li>
	</ul>
	</li>
	<li>Các lũy thừa trong LHS phải được sắp xếp theo <strong>thứ tự giảm dần</strong>.</li>
</ul>

<p>Hãy viết lời giải để xây dựng phương trình.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Terms:
+-------+--------+
| power | factor |
+-------+--------+
| 2     | 1      |
| 1     | -4     |
| 0     | 2      |
+-------+--------+
<strong>Đầu ra:</strong>
+--------------+
| equation     |
+--------------+
| +1X^2-4X+2=0 |
+--------------+
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Terms:
+-------+--------+
| power | factor |
+-------+--------+
| 4     | -4     |
| 2     | 1      |
| 1     | -1     |
+-------+--------+
<strong>Đầu ra:</strong>
+-----------------+
| equation        |
+-----------------+
| -4X^4+1X^2-1X=0 |
+-----------------+
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu power không phải là khóa chính nhưng mỗi power phải là duy nhất trong đáp án, bạn cần thay đổi gì trong lời giải?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần in đa thức theo thứ tự lũy thừa từ cao xuống thấp, với cách biểu diễn khác nhau cho hằng số, hạng tử bậc nhất và các hạng tử bậc cao hơn, đồng thời thêm dấu cộng trước các hệ số dương. Nếu tự xây dựng chuỗi trong application code, rất dễ xử lý sai.
>
> Dùng $\texttt{CASE}$ trên $\textit{power}$ để định dạng từng hạng tử: chỉ có hệ số với bậc $0$, có hệ số và `X` với bậc $1$, còn lại thêm `X^` cùng số mũ, đồng thời thêm dấu `+` khi factor dương.
>
> Dùng $\texttt{GROUP\_CONCAT}$ theo power giảm dần, rồi thêm `=0` vào cuối để tạo ra phương trình.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            power,
            CASE power
                WHEN 0 THEN IF(factor > 0, CONCAT('+', factor), factor)
                WHEN 1 THEN CONCAT(
                    IF(factor > 0, CONCAT('+', factor), factor),
                    'X'
                )
                ELSE CONCAT(
                    IF(factor > 0, CONCAT('+', factor), factor),
                    'X^',
                    power
                )
            END AS it
        FROM Terms
    )
SELECT
    CONCAT(GROUP_CONCAT(it ORDER BY power DESC SEPARATOR ""), '=0') AS equation
FROM T;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
