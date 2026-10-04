---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3126. Server Utilization Time 🔒](https://leetcode.com/problems/server-utilization-time)

[中文文档](/solution/3100-3199/3126.Server%20Utilization%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Servers</code></p>

<pre>
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| server_id      | int      |
| status_time    | datetime |
| session_status | enum     |
+----------------+----------+
(server_id, status_time, session_status) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
session_status là kiểu ENUM (phân loại) gồm (&#39;start&#39;, &#39;stop&#39;).
Mỗi hàng của bảng này chứa server_id, status_time và session_status.
</pre>

<p>Hãy viết lời giải để tìm <strong>tổng thời gian</strong> các server <strong>đang chạy</strong>. Kết quả cần được làm tròn xuống số <strong>ngày trọn vẹn</strong> gần nhất.</p>

<p>Trả về <em>bảng kết quả theo <strong>bất kỳ</strong></em><em>&nbsp;thứ tự nào.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Servers:</p>

<pre class="example-io">
+-----------+---------------------+----------------+
| server_id | status_time         | session_status |
+-----------+---------------------+----------------+
| 3         | 2023-11-04 16:29:47 | start          |
| 3         | 2023-11-05 01:49:47 | stop           |
| 3         | 2023-11-25 01:37:08 | start          |
| 3         | 2023-11-25 03:50:08 | stop           |
| 1         | 2023-11-13 03:05:31 | start          |
| 1         | 2023-11-13 11:10:31 | stop           |
| 4         | 2023-11-29 15:11:17 | start          |
| 4         | 2023-11-29 15:42:17 | stop           |
| 4         | 2023-11-20 00:31:44 | start          |
| 4         | 2023-11-20 07:03:44 | stop           |
| 1         | 2023-11-20 00:27:11 | start          |
| 1         | 2023-11-20 01:41:11 | stop           |
| 3         | 2023-11-04 23:16:48 | start          |
| 3         | 2023-11-05 01:15:48 | stop           |
| 4         | 2023-11-30 15:09:18 | start          |
| 4         | 2023-11-30 20:48:18 | stop           |
| 4         | 2023-11-25 21:09:06 | start          |
| 4         | 2023-11-26 04:58:06 | stop           |
| 5         | 2023-11-16 19:42:22 | start          |
| 5         | 2023-11-16 21:08:22 | stop           |
+-----------+---------------------+----------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------------+
| total_uptime_days |
+-------------------+
| 1                 |
+-------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đối với server ID 3:
	<ul>
		<li>Từ 2023-11-04 16:29:47 đến 2023-11-05 01:49:47: ~9.3 giờ</li>
		<li>Từ 2023-11-25 01:37:08 đến 2023-11-25 03:50:08: ~2.2 giờ</li>
		<li>Từ 2023-11-04 23:16:48 đến 2023-11-05 01:15:48: ~1.98 giờ</li>
	</ul>
	Tổng thời gian của server 3: ~13.48 giờ</li>
	<li>Đối với server ID 1:
	<ul>
		<li>Từ 2023-11-13 03:05:31 đến 2023-11-13 11:10:31: ~8 giờ</li>
		<li>Từ 2023-11-20 00:27:11 đến 2023-11-20 01:41:11: ~1.23 giờ</li>
	</ul>
	Tổng thời gian của server 1: ~9.23 giờ</li>
	<li>Đối với server ID 4:
	<ul>
		<li>Từ 2023-11-29 15:11:17 đến 2023-11-29 15:42:17: ~0.52 giờ</li>
		<li>Từ 2023-11-20 00:31:44 đến 2023-11-20 07:03:44: ~6.53 giờ</li>
		<li>Từ 2023-11-30 15:09:18 đến 2023-11-30 20:48:18: ~5.65 giờ</li>
		<li>Từ 2023-11-25 21:09:06 đến 2023-11-26 04:58:06: ~7.82 giờ</li>
	</ul>
	Tổng thời gian của server 4: ~20.52 giờ</li>
	<li>Đối với server ID 5:
	<ul>
		<li>Từ 2023-11-16 19:42:22 đến 2023-11-16 21:08:22: ~1.43 giờ</li>
	</ul>
	Tổng thời gian của server 5: ~1.43 giờ</li>
</ul>
Thời gian chạy tích lũy của tất cả server là khoảng 44.46 giờ, tương đương một ngày trọn vẹn và thêm một vài giờ. Tuy nhiên, vì chỉ tính các ngày trọn vẹn, kết quả cuối cùng được làm tròn thành 1 ngày trọn vẹn.</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Các dòng start/stop ghép thành các khoảng thời gian hoạt động; đáp án là tổng số giây chia cho $86400$. Nếu dùng self-join, cần sắp xếp cẩn thận theo từng server.
>
> `LEAD(status_time)` được phân vùng theo $server\_id$ sẽ trả về timestamp tiếp theo mà không cần self-join.
>
> Tính thời điểm tiếp theo, giữ lại các khoảng có trạng thái là `start`, cộng chênh lệch theo giây, rồi chia lấy phần nguyên cho độ dài một ngày.

<!-- thinking:end -->

Chúng ta có thể sử dụng hàm cửa sổ `LEAD` để lấy thời điểm của trạng thái tiếp theo cho mỗi server. Khoảng thời gian giữa hai trạng thái chính là thời gian server chạy. Cuối cùng, cộng thời gian chạy của tất cả server rồi chia cho số giây trong một ngày để tính tổng số ngày chạy của các server.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            session_status,
            status_time,
            LEAD(status_time) OVER (
                PARTITION BY server_id
                ORDER BY status_time
            ) AS next_status_time
        FROM Servers
    )
SELECT FLOOR(SUM(TIMESTAMPDIFF(SECOND, status_time, next_status_time)) / 86400) AS total_uptime_days
FROM T
WHERE session_status = 'start';
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
