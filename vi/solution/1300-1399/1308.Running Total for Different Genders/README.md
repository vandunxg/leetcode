---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1308. Running Total for Different Genders 🔒](https://leetcode.com/problems/running-total-for-different-genders)

[中文文档](/solution/1300-1399/1308.Running%20Total%20for%20Different%20Genders/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Scores</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| player_name   | varchar |
| gender        | varchar |
| day           | date    |
| score_points  | int     |
+---------------+---------+
(gender, day) là khóa chính của bảng này (tổ hợp cột có giá trị duy nhất).
Cuộc thi được tổ chức giữa đội nữ và đội nam.
Mỗi hàng trong bảng cho biết một người chơi có tên `player_name` và giới tính tương ứng đã ghi được `score_points` vào một ngày nào đó.
Giới tính là &#39;F&#39; nếu người chơi thuộc đội nữ và &#39;M&#39; nếu thuộc đội nam.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải tìm tổng điểm của mỗi giới tính vào từng ngày.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>gender</code> rồi đến <code>day</code> theo <strong>thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Scores:
+-------------+--------+------------+--------------+
| player_name | gender | day        | score_points |
+-------------+--------+------------+--------------+
| Aron        | F      | 2020-01-01 | 17           |
| Alice       | F      | 2020-01-07 | 23           |
| Bajrang     | M      | 2020-01-07 | 7            |
| Khali       | M      | 2019-12-25 | 11           |
| Slaman      | M      | 2019-12-30 | 13           |
| Joe         | M      | 2019-12-31 | 3            |
| Jose        | M      | 2019-12-18 | 2            |
| Priya       | F      | 2019-12-31 | 23           |
| Priyanka    | F      | 2019-12-30 | 17           |
+-------------+--------+------------+--------------+
<strong>Đầu ra:</strong> 
+--------+------------+-------+
| gender | day        | total |
+--------+------------+-------+
| F      | 2019-12-30 | 17    |
| F      | 2019-12-31 | 40    |
| F      | 2020-01-01 | 57    |
| F      | 2020-01-07 | 80    |
| M      | 2019-12-18 | 2     |
| M      | 2019-12-25 | 13    |
| M      | 2019-12-30 | 26    |
| M      | 2019-12-31 | 29    |
| M      | 2020-01-07 | 36    |
+--------+------------+-------+
<strong>Giải thích:</strong> 
Đối với đội nữ:
Ngày đầu tiên là 2019-12-30, Priyanka ghi 17 điểm và tổng điểm của đội là 17.
Ngày thứ hai là 2019-12-31, Priya ghi 23 điểm và tổng điểm của đội là 40.
Ngày thứ ba là 2020-01-01, Aron ghi 17 điểm và tổng điểm của đội là 57.
Ngày thứ tư là 2020-01-07, Alice ghi 23 điểm và tổng điểm của đội là 80.

Đối với đội nam:
Ngày đầu tiên là 2019-12-18, Jose ghi 2 điểm và tổng điểm của đội là 2.
Ngày thứ hai là 2019-12-25, Khali ghi 11 điểm và tổng điểm của đội là 13.
Ngày thứ ba là 2019-12-30, Slaman ghi 13 điểm và tổng điểm của đội là 26.
Ngày thứ tư là 2019-12-31, Joe ghi 3 điểm và tổng điểm của đội là 29.
Ngày thứ năm là 2020-01-07, Bajrang ghi 7 điểm và tổng điểm của đội là 36.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nếu tính lại từ đầu cho mỗi cặp giới tính và ngày, ta sẽ phải quét lại các hàng trước đó để có tổng điểm lũy kế. Hàm cửa sổ $\mathrm{SUM}$, phân vùng theo giới tính và sắp xếp theo ngày, cộng dồn điểm của tất cả các ngày trước đó cùng ngày hiện tại chỉ trong một lượt; đó chính là $\textit{total}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    gender,
    day,
    SUM(score_points) OVER (
        PARTITION BY gender
        ORDER BY gender, day
    ) AS total
FROM Scores;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
