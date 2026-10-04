---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3124. Find Longest Calls 🔒](https://leetcode.com/problems/find-longest-calls)

[中文文档](/solution/3100-3199/3124.Find%20Longest%20Calls/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Contacts</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| first_name  | varchar |
| last_name   | varchar |
+-------------+---------+
id là khóa chính (cột có các giá trị duy nhất) của bảng này.
id là khóa ngoại (cột tham chiếu) đến bảng <code>Calls</code>.
Mỗi dòng của bảng này chứa id, first_name và last_name.
</pre>

<p>Bảng: <code>Calls</code></p>

<pre>
+------------+------+
| Column Name | Type |
+------------+------+
| contact_id  | int  |
| type        | enum |
| duration    | int  |
+------------+------+
(contact_id, type, duration) là khóa chính (cột có các giá trị duy nhất) của bảng này.
type là kiểu ENUM (danh mục) gồm (&#39;incoming&#39;, &#39;outgoing&#39;).
Mỗi dòng của bảng này chứa thông tin về cuộc gọi, gồm contact_id, type và duration tính bằng giây.
</pre>

<p>Hãy viết lời giải để tìm <b>ba cuộc gọi dài nhất&nbsp;</b><strong>incoming</strong> và <strong>outgoing</strong>.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>type</code>, <code>duration</code> và<code> first_name</code>&nbsp;<em>theo <strong>thứ tự giảm dần</strong>, đồng thời <code>duration</code> phải được định dạng theo dạng <strong>HH:MM:SS</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Contacts:</p>

<pre class="example-io">
+----+------------+-----------+
| id | first_name | last_name |
+----+------------+-----------+
| 1  | John       | Doe       |
| 2  | Jane       | Smith     |
| 3  | Alice      | Johnson   |
| 4  | Michael    | Brown     |
| 5  | Emily      | Davis     |
+----+------------+-----------+
</pre>

<p>Bảng Calls:</p>

<pre class="example-io">
+------------+----------+----------+
| contact_id | type     | duration |
+------------+----------+----------+
| 1          | incoming | 120      |
| 1          | outgoing | 180      |
| 2          | incoming | 300      |
| 2          | outgoing | 240      |
| 3          | incoming | 150      |
| 3          | outgoing | 360      |
| 4          | incoming | 420      |
| 4          | outgoing | 200      |
| 5          | incoming | 180      |
| 5          | outgoing | 280      |
+------------+----------+----------+
        </pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-----------+----------+-------------------+
| first_name| type     | duration_formatted|
+-----------+----------+-------------------+
| Alice     | outgoing | 00:06:00          |
| Emily     | outgoing | 00:04:40          |
| Jane      | outgoing | 00:04:00          |
| Michael   | incoming | 00:07:00          |
| Jane      | incoming | 00:05:00          |
| Emily     | incoming | 00:03:00          |
+-----------+----------+-------------------+
        </pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Alice có một cuộc gọi outgoing kéo dài 6 phút.</li>
	<li>Emily có một cuộc gọi outgoing kéo dài 4 phút 40 giây.</li>
	<li>Jane có một cuộc gọi outgoing kéo dài 4 phút.</li>
	<li>Michael có một cuộc gọi incoming kéo dài 7 phút.</li>
	<li>Jane có một cuộc gọi incoming kéo dài 5 phút.</li>
	<li>Emily có một cuộc gọi incoming kéo dài 3 phút.</li>
</ul>

<p><b>Lưu ý:</b> Bảng kết quả được sắp xếp theo type, duration và first_name theo thứ tự giảm dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + Hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán yêu cầu lấy ba cuộc gọi dài nhất theo từng type, với duration được định dạng theo dạng `HH:MM:SS`. Có thể sắp xếp thủ công, nhưng khi có giá trị bằng nhau thì cần dùng dense rank.
>
> Thực hiện equi-join giữa contacts và calls, xếp hạng $duration$ theo thứ tự giảm dần trong từng type, rồi giữ lại các dòng có thứ hạng không vượt quá $3$.
>
> Chuyển số giây thành chuỗi thời gian, sau đó sắp xếp theo type, duration đã định dạng và tên. Window rank thay thế việc sắp xếp riêng theo từng nhóm.

<!-- thinking:end -->

Chúng ta có thể sử dụng equi-join để kết nối hai bảng, sau đó dùng hàm cửa sổ `RANK()` để tính thứ hạng của từng cuộc gọi theo type. Cuối cùng, chỉ cần lọc ra ba cuộc gọi đứng đầu.

<!-- tabs:start -->

#### MySQL

```sql
WITH
    T AS (
        SELECT
            first_name,
            type,
            DATE_FORMAT(SEC_TO_TIME(duration), "%H:%i:%s") AS duration_formatted,
            RANK() OVER (
                PARTITION BY type
                ORDER BY duration DESC
            ) AS rk
        FROM
            Calls AS c1
            JOIN Contacts AS c2 ON c1.contact_id = c2.id
    )
SELECT
    first_name,
    type,
    duration_formatted
FROM T
WHERE rk <= 3
ORDER BY 2, 3 DESC, 1 DESC;
```

#### Python3

```python
import pandas as pd


def find_longest_calls(contacts: pd.DataFrame, calls: pd.DataFrame) -> pd.DataFrame:
    merged_data = calls.merge(contacts, left_on="contact_id", right_on="id")
    merged_data["duration_formatted"] = (
        merged_data["duration"] // 3600 * 10000
        + merged_data["duration"] % 3600 // 60 * 100
        + merged_data["duration"] % 60
    ).apply(lambda x: "{:02}:{:02}:{:02}".format(x // 10000, x // 100 % 100, x % 100))

    merged_data["rk"] = merged_data.groupby("type")["duration"].rank(
        method="dense", ascending=False
    )

    result = merged_data[merged_data["rk"] <= 3][
        ["first_name", "type", "duration_formatted"]
    ]
    result = result.sort_values(
        by=["type", "duration_formatted", "first_name"], ascending=[True, False, False]
    )
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
