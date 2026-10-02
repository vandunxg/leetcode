---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [603. Consecutive Available Seats 🔒](https://leetcode.com/problems/consecutive-available-seats)

[中文文档](/solution/0600-0699/0603.Consecutive%20Available%20Seats/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Cinema</code></p>

<pre>
+-------------+------+
| Tên cột    | Kiểu |
+-------------+------+
| seat_id     | int  |
| free        | bool |
+-------------+------+
seat_id là cột tự tăng của bảng này.
Mỗi hàng cho biết ghế thứ i có còn trống hay không. 1 nghĩa là còn trống, còn 0 nghĩa là đã có người ngồi.
</pre>

<p>&nbsp;</p>

<p>Tìm tất cả các ghế trống liên tiếp trong rạp chiếu phim.</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>seat_id</code> <strong>tăng dần</strong>.</p>

<p>Bộ test được tạo sao cho có ít nhất ba ghế trống liên tiếp.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Cinema:
+---------+------+
| seat_id | free |
+---------+------+
| 1       | 1    |
| 2       | 0    |
| 3       | 1    |
| 4       | 1    |
| 5       | 1    |
+---------+------+
<strong>Đầu ra:</strong> 
+---------+
| seat_id |
+---------+
| 3       |
| 4       |
| 5       |
+---------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Self join

<!-- thinking:start -->

> **Tư duy**
>
> Một ghế trống chỉ được tính nếu có ghế trống liền kề. Truy vấn riêng các ghế kề nhau sẽ lặp lại việc kiểm tra.
>
> Self join các ghế có ID chênh nhau $1$ và đều còn trống; các ID riêng biệt trong kết quả join chính là đáp án.

<!-- thinking:end -->

Ta có thể self join bảng `Seat` với chính nó, rồi lọc các bản ghi sao cho `id` của ghế bên trái bằng `id` của ghế bên phải trừ $1$, đồng thời cả hai ghế đều trống.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DISTINCT a.seat_id
FROM
    Cinema AS a
    JOIN Cinema AS b ON ABS(a.seat_id - b.seat_id) = 1 AND a.free AND b.free
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Window function

<!-- thinking:start -->

> **Tư duy**
>
> Self join tạo ra các cặp ghế. `LAG`/`LEAD` lấy trạng thái `free` của ghế liền trước/liền sau ngay trên cùng hàng; nếu ghế hiện tại cộng với một trong hai ghế kề có tổng bằng $2$, ghế đó thuộc một cặp ghế trống liên tiếp.

<!-- thinking:end -->

Ta có thể dùng các hàm `LAG` và `LEAD` (hoặc `SUM() OVER(ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING)`) để lấy trạng thái của các ghế kề nhau, sau đó lọc các ghế trống liên tiếp và sắp xếp kết quả theo một thứ tự duy nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            seat_id,
            (free + (LAG(free) OVER (ORDER BY seat_id))) AS a,
            (free + (LEAD(free) OVER (ORDER BY seat_id))) AS b
        FROM Cinema
    )
SELECT seat_id
FROM T
WHERE a = 2 OR b = 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Một cửa sổ `SUM(free)` duy nhất trên hàng trước, hàng hiện tại và hàng sau thay cho các hàm `LAG`/`LEAD` riêng biệt. Ghế trống có tổng trong cửa sổ lớn hơn $1$ sẽ có ít nhất một ghế trống liền kề.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            SUM(free = 1) OVER (
                ORDER BY seat_id
                ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
            ) AS cnt
        FROM Cinema
    )
SELECT seat_id
FROM T
WHERE free = 1 AND cnt > 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
