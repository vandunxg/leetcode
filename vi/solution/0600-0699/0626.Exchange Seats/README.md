---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [626. Exchange Seats](https://leetcode.com/problems/exchange-seats)

[中文文档](/solution/0600-0699/0626.Exchange%20Seats/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Seat</code></p>

<pre>
+-------------+---------+
| Tên cột    | Kiểu dữ liệu |
+-------------+---------+
| id          | int     |
| student     | varchar |
+-------------+---------+
id là cột khóa chính (có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cho biết tên và ID của một sinh viên.
Dãy ID luôn bắt đầu từ 1 và tăng liên tục.
</pre>

<p>&nbsp;</p>

<p>Hãy viết truy vấn đổi seat id cho từng cặp sinh viên ngồi liền nhau. Nếu số sinh viên lẻ, giữ nguyên id của sinh viên cuối cùng.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>id</code> <strong>tăng dần</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Seat:
+----+---------+
| id | student |
+----+---------+
| 1  | Abbot   |
| 2  | Doris   |
| 3  | Emerson |
| 4  | Green   |
| 5  | Jeames  |
+----+---------+
<strong>Đầu ra:</strong> 
+----+---------+
| id | student |
+----+---------+
| 1  | Doris   |
| 2  | Abbot   |
| 3  | Green   |
| 4  | Emerson |
| 5  | Jeames  |
+----+---------+
<strong>Giải thích:</strong> 
Lưu ý, nếu số sinh viên lẻ thì không cần đổi chỗ ngồi của sinh viên cuối cùng.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các ghế có ID lẻ/chẵn liền kề sẽ đổi chỗ, còn ghế cuối lẻ sẽ giữ nguyên. Có thể dùng self join để lấy tên sinh viên ở ghế ghép cặp.
>
> `(id+1)^1-1` ánh xạ ID lẻ sang ID chẵn kế tiếp và ID chẵn sang ID lẻ liền trước. `COALESCE` giữ tên ban đầu nếu phép join không tìm thấy hàng ghép cặp.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT s1.id, COALESCE(s2.student, s1.student) AS student
FROM
    Seat AS s1
    LEFT JOIN Seat AS s2 ON (s1.id + 1) ^ 1 - 1 = s2.id
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Thay vì join, tính lại `id`: ID lẻ (không phải ID cuối) thì cộng một, ID chẵn thì trừ một, còn ID lẻ cuối cùng giữ nguyên; sau đó sắp xếp theo ID mới.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    id + (
        CASE
            WHEN id % 2 = 1
            AND id != (SELECT MAX(id) FROM Seat) THEN 1
            WHEN id % 2 = 0 THEN -1
            ELSE 0
        END
    ) AS id,
    student
FROM Seat
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Dùng XOR để đảo bit chỉ số bắt đầu từ $0$, rồi gọi `RANK` theo thứ tự đó để đổi từng cặp chỉ bằng một biểu thức; hàng cuối còn dư vẫn giữ nguyên vị trí tương đối.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    RANK() OVER (ORDER BY (id - 1) ^ 1) AS id,
    student
FROM Seat;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 4

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 dùng subquery để lấy `MAX(id)`. So sánh `ROW_NUMBER()` với `COUNT` dạng window giúp xác định hàng cuối mà không cần quét bảng lần thứ hai.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    CASE
        WHEN id & 1 = 0 THEN id - 1
        WHEN ROW_NUMBER() OVER (ORDER BY id) != COUNT(id) OVER () THEN id + 1
        ELSE id
    END AS id,
    student
FROM Seat
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
