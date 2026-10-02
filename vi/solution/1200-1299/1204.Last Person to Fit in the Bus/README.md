---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1204. Last Person to Fit in the Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus)

[中文文档](/solution/1200-1299/1204.Last%20Person%20to%20Fit%20in%20the%20Bus/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Queue</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |
+-------------+---------+
Cột person_id chứa các giá trị duy nhất.
Bảng này chứa thông tin về tất cả những người đang chờ xe buýt.
Các cột person_id và turn chứa đủ các số từ 1 đến n, trong đó n là số hàng của bảng.
turn xác định thứ tự lên xe: turn=1 là người lên đầu tiên và turn=n là người lên cuối cùng.
weight là trọng lượng của người đó tính bằng kilogram.
</pre>

<p>&nbsp;</p>

<p>Có một hàng người đang chờ lên xe buýt. Tuy nhiên, xe có giới hạn trọng lượng là <code>1000</code><strong> kilogram</strong>, nên có thể có người không thể lên xe.</p>

<p>Viết lời giải để tìm <code>person_name</code> của <strong>người cuối cùng</strong> có thể lên xe mà không vượt quá giới hạn trọng lượng. Các test case được tạo sao cho người đầu tiên không vượt giới hạn.</p>

<p><strong>Lưu ý</strong>, mỗi lượt chỉ có <em>một</em> người được lên xe.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Queue:
+-----------+-------------+--------+------+
| person_id | person_name | weight | turn |
+-----------+-------------+--------+------+
| 5         | Alice       | 250    | 1    |
| 4         | Bob         | 175    | 5    |
| 3         | Alex        | 350    | 2    |
| 6         | John Cena   | 400    | 3    |
| 1         | Winston     | 500    | 6    |
| 2         | Marie       | 200    | 4    |
+-----------+-------------+--------+------+
<strong>Đầu ra:</strong> 
+-------------+
| person_name |
+-------------+
| John Cena   |
+-------------+
<strong>Giải thích:</strong> Để đơn giản, bảng dưới đây được sắp xếp theo turn.
+------+----+-----------+--------+--------------+
| Lượt | ID | Tên       | Trọng lượng | Tổng trọng lượng |
+------+----+-----------+--------+--------------+
| 1    | 5  | Alice     | 250    | 250          |
| 2    | 3  | Alex      | 350    | 600          |
| 3    | 6  | John Cena | 400    | 1000         | (người cuối cùng lên xe)
| 4    | 2  | Marie     | 200    | 1200         | (không thể lên xe)
| 5    | 4  | Bob       | 175    | ___          |
| 6    | 1  | Winston   | 500    | ___          |
+------+----+-----------+--------+--------------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự lên xe được xác định bởi $turn$. Một người có thể lên xe khi tổng trọng lượng tiền tố tính đến người đó không vượt quá $1000$. Self-join ghép mỗi người với tất cả những người lên xe trước hoặc cùng lượt; $HAVING$ giữ lại các trường hợp chưa vượt giới hạn, rồi chọn $turn$ lớn nhất để tìm người cuối cùng có thể lên xe.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT a.person_name
FROM
    Queue AS a,
    Queue AS b
WHERE a.turn >= b.turn
GROUP BY a.person_id
HAVING SUM(b.weight) <= 1000
ORDER BY a.turn DESC
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Self-join tạo ra số cặp tăng theo bình phương. Tính tổng trọng lượng bằng window function theo thứ tự $turn$ sẽ cho biết tổng trọng lượng khi mỗi người lên xe chỉ trong một lượt duyệt; sau đó lọc và lấy tên ứng với tiền tố lớn nhất vẫn thỏa giới hạn. Cách này cho cùng kết quả như Lời giải 1 nhưng query đơn giản hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            person_name,
            SUM(weight) OVER (ORDER BY turn) AS s
        FROM Queue
    )
SELECT person_name
FROM T
WHERE s <= 1000
ORDER BY s DESC
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
