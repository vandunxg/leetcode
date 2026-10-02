---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [618. Students Report By Geography 🔒](https://leetcode.com/problems/students-report-by-geography)

[中文文档](/solution/0600-0699/0618.Students%20Report%20By%20Geography/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Student</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
+-------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Mỗi hàng trong bảng cho biết tên một học sinh và châu lục mà học sinh đó đến từ.
</pre>

<p>&nbsp;</p>

<p>Một trường học có học sinh đến từ châu Á, châu Âu và châu Mỹ.</p>

<p>Hãy viết lời giải để <a href="https://en.wikipedia.org/wiki/Pivot_table" target="_blank">pivot</a> cột continent trong bảng <code>Student</code>, sao cho tên của mỗi châu lục được <strong>sắp xếp theo thứ tự alphabet</strong> và hiển thị bên dưới châu lục tương ứng. Các header của kết quả lần lượt là <code>America</code>, <code>Asia</code> và <code>Europe</code>.</p>

<p>Các test case được tạo sao cho số học sinh đến từ châu Mỹ không ít hơn số học sinh đến từ châu Á hoặc châu Âu.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Student:
+--------+-----------+
| name   | continent |
+--------+-----------+
| Jane   | America   |
| Pascal | Europe    |
| Xi     | Asia      |
| Jack   | America   |
+--------+-----------+
<strong>Đầu ra:</strong> 
+---------+------+--------+
| America | Asia | Europe |
+---------+------+--------+
| Jack    | Xi   | Pascal |
| Jane    | null | null   |
+---------+------+--------+
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu không biết châu lục nào có nhiều học sinh nhất, bạn có thể viết lời giải để tạo báo cáo học sinh không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Học sinh từ ba châu lục cần được hiển thị cạnh nhau và sắp xếp theo tên. Conditional aggregation cần một chỉ số hàng chung.
>
> Dùng `ROW_NUMBER()` trong từng châu lục, sau đó `GROUP BY` theo thứ hạng đó cùng với `MAX(IF(continent=...))` để pivot tên học sinh thành ba cột.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            ROW_NUMBER() OVER (
                PARTITION BY continent
                ORDER BY name
            ) AS rk
        FROM Student
    )
SELECT
    MAX(IF(continent = 'America', name, NULL)) AS 'America',
    MAX(IF(continent = 'Asia', name, NULL)) AS 'Asia',
    MAX(IF(continent = 'Europe', name, NULL)) AS 'Europe'
FROM T
GROUP BY rk;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
