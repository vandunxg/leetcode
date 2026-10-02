---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [512. Game Play Analysis II 🔒](https://leetcode.com/problems/game-play-analysis-ii)

[中文文档](/solution/0500-0599/0512.Game%20Play%20Analysis%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Activity</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
(player_id, event_date) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng này ghi lại hoạt động của người chơi trong một số trò chơi.
Mỗi hàng ghi lại một lần người chơi đăng nhập bằng một thiết bị, chơi một số ván (có thể là 0), rồi đăng xuất vào một ngày nào đó.
</pre>

<p>&nbsp;</p>

<p>Viết truy vấn để tìm <strong>thiết bị</strong> được mỗi người chơi dùng trong lần đăng nhập đầu tiên.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Activity:
+-----------+-----------+------------+--------------+
| player_id | device_id | event_date | games_played |
+-----------+-----------+------------+--------------+
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-05-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |
+-----------+-----------+------------+--------------+
<strong>Đầu ra:</strong> 
+-----------+-----------+
| player_id | device_id |
+-----------+-----------+
| 1         | 2         |
| 2         | 3         |
| 3         | 1         |
+-----------+-----------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Subquery

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi tìm ngày đăng nhập đầu tiên, ta vẫn cần lấy `device_id` của ngày đó. Nếu group theo thiết bị thì không đảm bảo thiết bị tương ứng với ngày nhỏ nhất.
>
> Subquery tính `MIN(event_date)` cho từng người chơi; query bên ngoài khớp `(player_id, event_date)` để lấy lại thiết bị. Khóa chính ghép xác định chính xác hàng đăng nhập đầu tiên.

<!-- thinking:end -->

Ta có thể dùng `GROUP BY` và hàm `MIN` để tìm ngày đăng nhập đầu tiên của mỗi người chơi, sau đó dùng subquery với khóa ghép để tìm thiết bị đã dùng trong lần đăng nhập đó.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    player_id,
    device_id
FROM Activity
WHERE
    (player_id, event_date) IN (
        SELECT
            player_id,
            MIN(event_date) AS event_date
        FROM Activity
        GROUP BY 1
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Subquery thực hiện phép tổng hợp rồi truy vấn lại bảng. Window function có thể xếp hạng ngày đăng nhập của từng người chơi ngay trên bảng gốc rồi giữ hạng $1$.
>
> `RANK() OVER (PARTITION BY player_id ORDER BY event_date)` đánh dấu lần đăng nhập đầu tiên; điều kiện lọc bên ngoài loại các hàng còn lại. Kết quả giống lời giải 1 mà không cần self-join tường minh.

<!-- thinking:end -->

Ta có thể dùng window function `rank()` để xếp hạng ngày đăng nhập của từng người chơi, rồi chọn các hàng có hạng $1$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY player_id
                ORDER BY event_date
            ) AS rk
        FROM Activity
    )
SELECT player_id, device_id
FROM T
WHERE rk = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
