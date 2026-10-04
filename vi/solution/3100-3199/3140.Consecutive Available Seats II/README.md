---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3140. Consecutive Available Seats II 🔒](https://leetcode.com/problems/consecutive-available-seats-ii)

[中文文档](/solution/3100-3199/3140.Consecutive%20Available%20Seats%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Cinema</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| seat_id     | int  |
| free        | bool |
+-------------+------+
seat_id is an auto-increment column for this table.
Each row of this table indicates whether the i<sup>th</sup> seat is free or not. 1 means free while 0 means occupied.
</pre>

<p>Hãy viết lời giải để tìm <strong>độ dài</strong> của <strong>chuỗi liên tiếp dài nhất</strong> gồm các ghế <strong>còn trống</strong> trong rạp chiếu phim.</p>

<p>Lưu ý:</p>

<ul>
	<li>Sẽ luôn có <strong>nhiều nhất</strong> <strong>một</strong> chuỗi liên tiếp dài nhất.</li>
	<li>Nếu có <strong>nhiều</strong> chuỗi liên tiếp có <strong>cùng độ dài</strong>, hãy đưa tất cả chúng vào kết quả.</li>
</ul>

<p>Trả về <em>bảng kết quả được <strong>sắp xếp</strong> theo</em> <code>first_seat_id</code> <em><strong>theo thứ tự tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Cinema:</p>

<pre class="example-io">
+---------+------+
| seat_id | free |
+---------+------+
| 1       | 1    |
| 2       | 0    |
| 3       | 1    |
| 4       | 1    |
| 5       | 1    |
+---------+------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-----------------+----------------+-----------------------+
| first_seat_id   | last_seat_id   | consecutive_seats_len |
+-----------------+----------------+-----------------------+
| 3               | 5              | 3                     |
+-----------------+----------------+-----------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chuỗi liên tiếp dài nhất gồm các ghế còn trống bắt đầu từ ghế 3 và kết thúc ở ghế 5, có độ dài là 3.</li>
</ul>
Bảng kết quả được sắp xếp theo first_seat_id theo thứ tự tăng dần.</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm mọi đoạn liên tiếp dài nhất của các ghế còn trống. Có thể quét tuần tự, nhưng SQL tự nhiên hơn khi nhóm các chỉ số liên tiếp.
>
> Việc lấy thứ hạng của một ghế còn trống trừ khỏi $seat\_id$ cho một giá trị không đổi trong mỗi đoạn liên tiếp, và giá trị này trở thành khóa nhóm.
>
> Chỉ giữ lại $free=1$, nhóm theo $seat\_id-\mathrm{RANK}()$, tính giá trị nhỏ nhất, lớn nhất và độ dài, sau đó giữ các nhóm có độ dài lớn nhất và sắp xếp theo ghế đầu tiên.

<!-- thinking:end -->

Trước tiên, ta tìm tất cả các ghế còn trống, sau đó nhóm các ghế lại. Việc nhóm dựa trên số ghế trừ đi thứ hạng của nó. Nhờ vậy, các ghế còn trống liên tiếp sẽ được nhóm lại với nhau. Tiếp theo, ta tìm số ghế nhỏ nhất, số ghế lớn nhất và độ dài chuỗi ghế liên tiếp trong mỗi nhóm. Cuối cùng, ta tìm nhóm có chuỗi ghế liên tiếp dài nhất, rồi xuất số ghế nhỏ nhất, số ghế lớn nhất và độ dài chuỗi ghế liên tiếp trong nhóm này.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            seat_id - (RANK() OVER (ORDER BY seat_id)) AS gid
        FROM Cinema
        WHERE free = 1
    ),
    P AS (
        SELECT
            MIN(seat_id) AS first_seat_id,
            MAX(seat_id) AS last_seat_id,
            COUNT(1) AS consecutive_seats_len,
            RANK() OVER (ORDER BY COUNT(1) DESC) AS rk
        FROM T
        GROUP BY gid
    )
SELECT first_seat_id, last_seat_id, consecutive_seats_len
FROM P
WHERE rk = 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
