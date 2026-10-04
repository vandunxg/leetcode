---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3601. Find Drivers with Improved Fuel Efficiency](https://leetcode.com/problems/find-drivers-with-improved-fuel-efficiency)

[中文文档](/solution/3600-3699/3601.Find%20Drivers%20with%20Improved%20Fuel%20Efficiency/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>drivers</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| driver_id   | int     |
| driver_name | varchar |
+-------------+---------+
driver_id là định danh duy nhất của bảng này.
Mỗi dòng chứa thông tin về một tài xế.
</pre>

<p>Bảng: <code>trips</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| trip_id       | int     |
| driver_id     | int     |
| trip_date     | date    |
| distance_km   | decimal |
| fuel_consumed | decimal |
+---------------+---------+
trip_id là định danh duy nhất của bảng này.
Mỗi dòng biểu diễn một chuyến đi do một tài xế thực hiện, bao gồm quãng đường đã đi và lượng nhiên liệu tiêu thụ cho chuyến đi đó.
</pre>

<p>Hãy viết lời giải để tìm các tài xế có <strong>hiệu suất nhiên liệu được cải thiện</strong> bằng cách <strong>so sánh</strong> hiệu suất nhiên liệu trung bình của họ trong <strong>nửa đầu</strong> năm với <strong>nửa sau</strong> năm.</p>

<ul>
    <li>Tính <strong>hiệu suất nhiên liệu</strong> bằng <code>distance_km / fuel_consumed</code> cho <strong>từng</strong> chuyến đi</li>
    <li><strong>Nửa đầu</strong>: từ tháng Một đến tháng Sáu, <strong>nửa sau</strong>: từ tháng Bảy đến tháng Mười Hai</li>
    <li>Chỉ bao gồm các tài xế có chuyến đi trong <strong>cả hai nửa</strong> của năm</li>
    <li>Tính <strong>mức cải thiện hiệu suất</strong> bằng (<code>second_half_avg - first_half_avg</code>)</li>
    <li><strong>Làm tròn </strong>tất cả<strong> </strong>kết quả<strong> </strong>đến<strong> <code>2</code> </strong>chữ số<strong> </strong>thập phân</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo mức cải thiện hiệu suất theo thứ tự <strong>giảm dần</strong>, sau đó theo tên tài xế theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng drivers:</p>

<pre class="example-io">
+-----------+---------------+
| driver_id | driver_name   |
+-----------+---------------+
| 1         | Alice Johnson |
| 2         | Bob Smith     |
| 3         | Carol Davis   |
| 4         | David Wilson  |
| 5         | Emma Brown    |
+-----------+---------------+
</pre>

<p>Bảng trips:</p>

<pre class="example-io">
+---------+-----------+------------+-------------+---------------+
| trip_id | driver_id | trip_date  | distance_km | fuel_consumed |
+---------+-----------+------------+-------------+---------------+
| 1       | 1         | 2023-02-15 | 120.5       | 10.2          |
| 2       | 1         | 2023-03-20 | 200.0       | 16.5          |
| 3       | 1         | 2023-08-10 | 150.0       | 11.0          |
| 4       | 1         | 2023-09-25 | 180.0       | 12.5          |
| 5       | 2         | 2023-01-10 | 100.0       | 9.0           |
| 6       | 2         | 2023-04-15 | 250.0       | 22.0          |
| 7       | 2         | 2023-10-05 | 200.0       | 15.0          |
| 8       | 3         | 2023-03-12 | 80.0        | 8.5           |
| 9       | 3         | 2023-05-18 | 90.0        | 9.2           |
| 10      | 4         | 2023-07-22 | 160.0       | 12.8          |
| 11      | 4         | 2023-11-30 | 140.0       | 11.0          |
| 12      | 5         | 2023-02-28 | 110.0       | 11.5          |
+---------+-----------+------------+-------------+---------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-----------+---------------+------------------+-------------------+------------------------+
| driver_id | driver_name   | first_half_avg   | second_half_avg   | efficiency_improvement |
+-----------+---------------+------------------+-------------------+------------------------+
| 2         | Bob Smith     | 11.24            | 13.33             | 2.10                   |
| 1         | Alice Johnson | 11.97            | 14.02             | 2.05                   |
+-----------+---------------+------------------+-------------------+------------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Alice Johnson (driver_id = 1):</strong>

    <ul>
        <li>Các chuyến đi trong nửa đầu (tháng Một - tháng Sáu): ngày 15 tháng Hai (120.5/10.2 = 11.81), ngày 20 tháng Ba (200.0/16.5 = 12.12)</li>
        <li>Hiệu suất trung bình trong nửa đầu: (11.81 + 12.12) / 2 = 11.97</li>
        <li>Các chuyến đi trong nửa sau (tháng Bảy - tháng Mười Hai): ngày 10 tháng Tám (150.0/11.0 = 13.64), ngày 25 tháng Chín (180.0/12.5 = 14.40)</li>
        <li>Hiệu suất trung bình trong nửa sau: (13.64 + 14.40) / 2 = 14.02</li>
        <li>Mức cải thiện hiệu suất: 14.02 - 11.97 = 2.05</li>
    </ul>
    </li>
    <li><strong>Bob Smith (driver_id = 2):</strong>
    <ul>
        <li>Các chuyến đi trong nửa đầu: ngày 10 tháng Một (100.0/9.0 = 11.11), ngày 15 tháng Tư (250.0/22.0 = 11.36)</li>
        <li>Hiệu suất trung bình trong nửa đầu: (11.11 + 11.36) / 2 = 11.24</li>
        <li>Các chuyến đi trong nửa sau: ngày 5 tháng Mười (200.0/15.0 = 13.33)</li>
        <li>Hiệu suất trung bình trong nửa sau: 13.33</li>
        <li>Mức cải thiện hiệu suất: 13.33 - 11.24 = 2.10 (làm tròn đến 2 chữ số thập phân)</li>
    </ul>
    </li>
    <li><strong>Các tài xế không được đưa vào:</strong>
    <ul>
        <li>Carol Davis (driver_id = 3): Chỉ có chuyến đi trong nửa đầu (tháng Ba, tháng Năm)</li>
        <li>David Wilson (driver_id = 4): Chỉ có chuyến đi trong nửa sau (tháng Bảy, tháng Mười Một)</li>
        <li>Emma Brown (driver_id = 5): Chỉ có chuyến đi trong nửa đầu (tháng Hai)</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo mức cải thiện hiệu suất theo thứ tự giảm dần, sau đó theo tên theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group Aggregation + Join Query

<!-- thinking:start -->

> **Tư duy**
>
> Việc quét các chuyến đi của từng tài xế rồi tự chia thành hai nửa rất dễ bỏ sót những tài xế thiếu một nửa năm, đồng thời dễ nhầm giữa tỷ lệ của tổng với giá trị trung bình của hiệu suất từng chuyến đi. Điều cần so sánh là hai giá trị trung bình theo nửa năm của cùng một tài xế.
>
> Ghi lại $\textit{distance}/\textit{fuel}$ cho từng chuyến đi, sau đó tính trung bình theo $\textit{driver\_id}$ và nửa năm. Sau khi pivot, loại bỏ các dòng thiếu một nửa và giữ lại những dòng có giá trị trung bình của nửa sau lớn hơn nghiêm ngặt.
>
> JOIN với $\textit{drivers}$ để lấy tên rồi sắp xếp theo mức cải thiện giảm dần, sau đó theo tên tăng dần. Việc group và pivot biến nửa năm thành một khóa tổng hợp tường minh.

<!-- thinking:end -->

Đầu tiên, chúng ta thực hiện gom nhóm trên bảng `trips` để tính hiệu suất nhiên liệu trung bình của từng tài xế trong nửa đầu và nửa sau của năm.

Sau đó, chúng ta JOIN kết quả với bảng `drivers`, lọc ra những tài xế có hiệu suất nhiên liệu được cải thiện và tính mức cải thiện.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            driver_id,
            AVG(distance_km / fuel_consumed) half_avg,
            CASE
                WHEN MONTH(trip_date) <= 6 THEN 1
                ELSE 2
            END half
        FROM trips
        GROUP BY driver_id, half
    )
SELECT
    t1.driver_id,
    d.driver_name,
    ROUND(t1.half_avg, 2) first_half_avg,
    ROUND(t2.half_avg, 2) second_half_avg,
    ROUND(t2.half_avg - t1.half_avg, 2) efficiency_improvement
FROM
    T t1
    JOIN T t2 ON t1.driver_id = t2.driver_id AND t1.half < t2.half AND t1.half_avg < t2.half_avg
    JOIN drivers d ON t1.driver_id = d.driver_id
ORDER BY efficiency_improvement DESC, d.driver_name;
```

#### Pandas

```python
import pandas as pd


def find_improved_efficiency_drivers(
    drivers: pd.DataFrame, trips: pd.DataFrame
) -> pd.DataFrame:
    trips = trips.copy()
    trips["trip_date"] = pd.to_datetime(trips["trip_date"])
    trips["half"] = trips["trip_date"].dt.month.apply(lambda m: 1 if m <= 6 else 2)
    trips["efficiency"] = trips["distance_km"] / trips["fuel_consumed"]
    half_avg = (
        trips.groupby(["driver_id", "half"])["efficiency"]
        .mean()
        .reset_index(name="half_avg")
    )
    pivot = half_avg.pivot(index="driver_id", columns="half", values="half_avg").rename(
        columns={1: "first_half_avg", 2: "second_half_avg"}
    )
    pivot = pivot.dropna()
    pivot = pivot[pivot["second_half_avg"] > pivot["first_half_avg"]]
    pivot["efficiency_improvement"] = (
        pivot["second_half_avg"] - pivot["first_half_avg"]
    ).round(2)
    pivot["first_half_avg"] = pivot["first_half_avg"].round(2)
    pivot["second_half_avg"] = pivot["second_half_avg"].round(2)
    result = pivot.reset_index().merge(drivers, on="driver_id")
    result = result.sort_values(
        by=["efficiency_improvement", "driver_name"], ascending=[False, True]
    )
    return result[
        [
            "driver_id",
            "driver_name",
            "first_half_avg",
            "second_half_avg",
            "efficiency_improvement",
        ]
    ]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
