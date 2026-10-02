---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1225. Report Contiguous Dates 🔒](https://leetcode.com/problems/report-contiguous-dates)

[中文文档](/solution/1200-1299/1225.Report%20Contiguous%20Dates/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Failed</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| fail_date    | date    |
+--------------+---------+
`fail_date` là khóa chính (cột có giá trị duy nhất) của bảng này.
Bảng này chứa các ngày tác vụ thất bại.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Succeeded</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| success_date | date    |
+--------------+---------+
`success_date` là khóa chính (cột có giá trị duy nhất) của bảng này.
Bảng này chứa các ngày tác vụ thành công.
</pre>

<p>&nbsp;</p>

<p>Hệ thống chạy một tác vụ <strong>mỗi ngày</strong>. Mỗi tác vụ độc lập với các tác vụ trước đó và có thể thành công hoặc thất bại.</p>

<p>Hãy viết lời giải báo cáo <code>period_state</code> cho từng khoảng ngày liên tiếp trong giai đoạn từ <code>2019-01-01</code> đến <code>2019-12-31</code>.</p>

<p><code>period_state</code> là <em>&#39;</em><code>failed&#39;</code><em> </em>nếu các tác vụ trong khoảng đó thất bại, hoặc là <code>&#39;succeeded&#39;</code> nếu các tác vụ thành công. Khoảng ngày được biểu diễn bằng <code>start_date</code> và <code>end_date.</code></p>

<p>Trả về bảng kết quả được sắp xếp theo <code>start_date</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Failed:
+-------------------+
| fail_date         |
+-------------------+
| 2018-12-28        |
| 2018-12-29        |
| 2019-01-04        |
| 2019-01-05        |
+-------------------+
Bảng Succeeded:
+-------------------+
| success_date      |
+-------------------+
| 2018-12-30        |
| 2018-12-31        |
| 2019-01-01        |
| 2019-01-02        |
| 2019-01-03        |
| 2019-01-06        |
+-------------------+
<strong>Đầu ra:</strong> 
+--------------+--------------+--------------+
| period_state | start_date   | end_date     |
+--------------+--------------+--------------+
| succeeded    | 2019-01-01   | 2019-01-03   |
| failed       | 2019-01-04   | 2019-01-05   |
| succeeded    | 2019-01-06   | 2019-01-06   |
+--------------+--------------+--------------+
<strong>Giải thích:</strong> 
Báo cáo bỏ qua trạng thái hệ thống trong năm 2018 vì ta chỉ quan tâm đến hệ thống trong giai đoạn 2019-01-01 đến 2019-12-31.
Từ 2019-01-01 đến 2019-01-03, tất cả tác vụ đều thành công và trạng thái hệ thống là &quot;succeeded&quot;.
Từ 2019-01-04 đến 2019-01-05, tất cả tác vụ đều thất bại và trạng thái hệ thống là &quot;failed&quot;.
Từ 2019-01-06 đến 2019-01-06, tất cả tác vụ đều thành công và trạng thái hệ thống là &quot;succeeded&quot;.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: UNION + hàm cửa sổ + GROUP BY

<!-- thinking:start -->

> **Tư duy**
>
> Ngày thành công và thất bại nằm ở hai bảng riêng, nhưng các khoảng liên tiếp cùng thuộc một dòng thời gian. Ta gộp ngày thất bại và thành công trong năm $2019$ thành một luồng sự kiện duy nhất, kèm cờ trạng thái.
>
> Trong một đoạn có cùng trạng thái, hiệu giữa ngày và thứ hạng trong nhóm là hằng số; hiệu này dùng làm nhãn cho đoạn. Nhóm theo trạng thái và nhãn đó, rồi lấy ngày $MIN$/$MAX$ để xác định hai đầu mỗi khoảng.
>
> $UNION\ ALL$ hợp nhất dữ liệu hai bảng, $RANK$ tạo khóa cho từng đoạn, còn $GROUP\ BY$ gộp các ngày trong mỗi khoảng.

<!-- thinking:end -->

Ta có thể gộp hai bảng thành một bảng với trường `st` biểu thị trạng thái: `failed` là thất bại, `succeeded` là thành công. Sau đó, dùng hàm cửa sổ để xếp các bản ghi cùng trạng thái vào nhóm và tính hiệu giữa mỗi ngày với thứ hạng của ngày đó trong nhóm, đặt là `pt`; giá trị này định danh các ngày liên tiếp có cùng trạng thái. Cuối cùng, nhóm theo `st` và `pt`, tính ngày nhỏ nhất và lớn nhất trong mỗi nhóm, rồi sắp xếp theo ngày nhỏ nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT fail_date AS dt, 'failed' AS st
        FROM Failed
        WHERE YEAR(fail_date) = 2019
        UNION ALL
        SELECT success_date AS dt, 'succeeded' AS st
        FROM Succeeded
        WHERE YEAR(success_date) = 2019
    )
SELECT
    st AS period_state,
    MIN(dt) AS start_date,
    MAX(dt) AS end_date
FROM
    (
        SELECT
            *,
            SUBDATE(
                dt,
                RANK() OVER (
                    PARTITION BY st
                    ORDER BY dt
                )
            ) AS pt
        FROM T
    ) AS t
GROUP BY 1, pt
ORDER BY 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
