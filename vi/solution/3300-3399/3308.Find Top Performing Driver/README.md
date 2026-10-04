---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3308. Find Top Performing Driver 🔒](https://leetcode.com/problems/find-top-performing-driver)

[中文文档](/solution/3300-3399/3308.Find%20Top%20Performing%20Driver/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace"><code>Drivers</code></font></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| driver_id    | int     |
| name         | varchar |
| age          | int     |
| experience   | int     |
| accidents    | int     |
+--------------+---------+
(driver_id) là khóa duy nhất của bảng này.
Mỗi dòng chứa ID của tài xế, tên, tuổi, số năm kinh nghiệm lái xe và số vụ tai nạn mà họ đã gặp.
</pre>

<p>Bảng: <font face="monospace"><code>Vehicles</code></font></p>

<pre>
+--------------+---------+
| vehicle_id   | int     |
| driver_id    | int     |
| model        | varchar |
| fuel_type    | varchar |
| mileage      | int     |
+--------------+---------+
(vehicle_id, driver_id, fuel_type) là khóa duy nhất của bảng này.
Mỗi dòng chứa ID của phương tiện, tài xế điều khiển phương tiện đó, model, loại nhiên liệu và quãng đường đã đi.
</pre>

<p>Bảng: <font face="monospace"><code>Trips</code></font></p>

<pre>
+--------------+---------+
| trip_id      | int     |
| vehicle_id   | int     |
| distance     | int     |
| duration     | int     |
| rating       | int     |
+--------------+---------+
(trip_id) là khóa duy nhất của bảng này.
Mỗi dòng chứa ID của chuyến đi, phương tiện được sử dụng, quãng đường đã đi (tính bằng dặm), thời lượng chuyến đi (tính bằng phút) và đánh giá của hành khách (1-5).
</pre>

<p>Uber đang phân tích các tài xế dựa trên những chuyến đi của họ. Hãy viết một lời giải để tìm <strong>tài xế có thành tích cao nhất</strong> cho <strong>mỗi loại nhiên liệu</strong> dựa trên các tiêu chí sau:</p>

<ol>
	<li>Thành tích của một tài xế được tính bằng <strong>điểm đánh giá trung bình</strong> trên tất cả các chuyến đi của họ. Điểm đánh giá trung bình phải được làm tròn đến <code>2</code> chữ số thập phân.</li>
	<li>Nếu hai tài xế có cùng điểm đánh giá trung bình, tài xế đã đi <strong>tổng quãng đường dài hơn</strong> được xếp hạng cao hơn.</li>
	<li>Nếu <strong>vẫn hòa</strong>, chọn tài xế có <strong>ít vụ tai nạn nhất</strong>.</li>
</ol>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>fuel_type</code> <em>theo thứ tự </em><strong>tăng dần</strong><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng <code>Drivers</code>:</p>

<pre class="example-io">
+-----------+----------+-----+------------+-----------+
| driver_id | name     | age | experience | accidents |
+-----------+----------+-----+------------+-----------+
| 1         | Alice    | 34  | 10         | 1         |
| 2         | Bob      | 45  | 20         | 3         |
| 3         | Charlie  | 28  | 5          | 0         |
+-----------+----------+-----+------------+-----------+
</pre>

<p>Bảng <code>Vehicles</code>:</p>

<pre class="example-io">
+------------+-----------+---------+-----------+---------+
| vehicle_id | driver_id | model   | fuel_type | mileage |
+------------+-----------+---------+-----------+---------+
| 100        | 1         | Sedan   | Gasoline  | 20000   |
| 101        | 2         | SUV     | Electric  | 30000   |
| 102        | 3         | Coupe   | Gasoline  | 15000   |
+------------+-----------+---------+-----------+---------+
</pre>

<p>Bảng <code>Trips</code>:</p>

<pre class="example-io">
+---------+------------+----------+----------+--------+
| trip_id | vehicle_id | distance | duration | rating |
+---------+------------+----------+----------+--------+
| 201     | 100        | 50       | 30       | 5      |
| 202     | 100        | 30       | 20       | 4      |
| 203     | 101        | 100      | 60       | 4      |
| 204     | 101        | 80       | 50       | 5      |
| 205     | 102        | 40       | 30       | 5      |
| 206     | 102        | 60       | 40       | 5      |
+---------+------------+----------+----------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-----------+-----------+--------+----------+
| fuel_type | driver_id | rating | distance |
+-----------+-----------+--------+----------+
| Electric  | 2         | 4.50   | 180      |
| Gasoline  | 3         | 5.00   | 100      |
+-----------+-----------+--------+----------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với loại nhiên liệu <code>Gasoline</code>, cả Alice (Tài xế 1) và Charlie (Tài xế 3) đều có chuyến đi. Charlie có điểm đánh giá trung bình là 5.0, còn Alice là 4.5. Vì vậy, Charlie được chọn.</li>
	<li>Với loại nhiên liệu <code>Electric</code>, Bob (Tài xế 2) là tài xế duy nhất có điểm đánh giá trung bình là 4.5, nên anh ấy được chọn.</li>
</ul>

<p>Bảng kết quả được sắp xếp theo <code>fuel_type</code> theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-join + Grouping + Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi loại nhiên liệu, ta cần tìm tài xế có điểm đánh giá tốt nhất, sau đó là tổng quãng đường lớn nhất, rồi đến số vụ tai nạn ít nhất; các trường hợp hòa vẫn được giữ lại. Việc nối $\textit{Drivers}$, $\textit{Vehicles}$ và $\textit{Trips}$ rồi nhóm theo loại nhiên liệu và tài xế sẽ tạo ra ba giá trị tổng hợp này.
>
> Một câu lệnh $\textit{GROUP BY}$ thông thường kết hợp với $\textit{MAX}$ không thể trả về mọi $\textit{driver\_id}$ bị hòa.
>
> Vì vậy, ta xếp hạng các giá trị tổng hợp theo điểm đánh giá giảm dần, quãng đường giảm dần và số vụ tai nạn tăng dần, giữ lại $\textit{rk}=1$, rồi sắp xếp theo loại nhiên liệu.

<!-- thinking:end -->

Ta có thể dùng equi-join để nối bảng `Drivers` với bảng `Vehicles` theo `driver_id`, sau đó nối với bảng `Trips` theo `vehicle_id`. Tiếp theo, ta nhóm theo `fuel_type` và `driver_id` để tính điểm đánh giá trung bình, tổng quãng đường và tổng số vụ tai nạn của mỗi tài xế. Sau đó, dùng hàm cửa sổ `RANK()`, ta xếp hạng các tài xế trong từng loại nhiên liệu theo điểm đánh giá giảm dần, tổng quãng đường giảm dần và tổng số vụ tai nạn tăng dần. Cuối cùng, ta lọc tài xế có hạng 1 trong mỗi loại nhiên liệu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            fuel_type,
            driver_id,
            ROUND(AVG(rating), 2) rating,
            SUM(distance) distance,
            SUM(accidents) accidents
        FROM
            Drivers
            JOIN Vehicles USING (driver_id)
            JOIN Trips USING (vehicle_id)
        GROUP BY fuel_type, driver_id
    ),
    P AS (
        SELECT
            *,
            RANK() OVER (
                PARTITION BY fuel_type
                ORDER BY rating DESC, distance DESC, accidents
            ) rk
        FROM T
    )
SELECT fuel_type, driver_id, rating, distance
FROM P
WHERE rk = 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
