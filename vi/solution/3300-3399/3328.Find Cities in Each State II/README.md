---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3328. Find Cities in Each State II 🔒](https://leetcode.com/problems/find-cities-in-each-state-ii)

[中文文档](/solution/3300-3399/3328.Find%20Cities%20in%20Each%20State%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>cities</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| state       | varchar |
| city        | varchar |
+-------------+---------+
(state, city) is the combination of columns with unique values for this table.
Each row of this table contains the state name and the city name within that state.
</pre>

<p>Hãy viết lời giải để tìm <strong>tất cả các thành phố</strong> trong <strong>từng bang</strong> và phân tích chúng theo các yêu cầu sau:</p>

<ul>
    <li>Gộp tất cả thành phố thành một chuỗi <strong>phân tách bằng dấu phẩy</strong> cho mỗi bang.</li>
    <li>Chỉ bao gồm các bang có <strong>ít nhất</strong> <code>3</code> thành phố.</li>
    <li>Chỉ bao gồm các bang có <strong>ít nhất một thành phố</strong> bắt đầu bằng <strong>cùng chữ cái với tên bang</strong>.</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <em>số lượng thành phố bắt đầu bằng chữ cái trùng khớp theo thứ tự <strong>giảm dần</strong></em>&nbsp;<em>và sau đó theo tên bang theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được thể hiện trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng cities:</p>

<pre class="example-io">
+--------------+---------------+
| state        | city          |
+--------------+---------------+
| New York     | New York City |
| New York     | Newark        |
| New York     | Buffalo       |
| New York     | Rochester     |
| California   | San Francisco |
| California   | Sacramento    |
| California   | San Diego     |
| California   | Los Angeles   |
| Texas        | Tyler         |
| Texas        | Temple        |
| Texas        | Taylor        |
| Texas        | Dallas        |
| Pennsylvania | Philadelphia  |
| Pennsylvania | Pittsburgh    |
| Pennsylvania | Pottstown     |
+--------------+---------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+-------------------------------------------+-----------------------+
| state       | cities                                    | matching_letter_count |
+-------------+-------------------------------------------+-----------------------+
| Pennsylvania| Philadelphia, Pittsburgh, Pottstown       | 3                     |
| Texas       | Dallas, Taylor, Temple, Tyler             | 3                     |
| New York    | Buffalo, Newark, New York City, Rochester | 2                     |
+-------------+-------------------------------------------+-----------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Pennsylvania</strong>:

    <ul>
        <li>Có 3 thành phố (đáp ứng yêu cầu tối thiểu)</li>
        <li>Cả 3 thành phố đều bắt đầu bằng &#39;P&#39; (giống tên bang)</li>
        <li>matching_letter_count = 3</li>
    </ul>
    </li>
    <li><strong>Texas</strong>:
    <ul>
        <li>Có 4 thành phố (đáp ứng yêu cầu tối thiểu)</li>
        <li>3 thành phố (Taylor, Temple, Tyler) bắt đầu bằng &#39;T&#39; (giống tên bang)</li>
        <li>matching_letter_count = 3</li>
    </ul>
    </li>
    <li><strong>New York</strong>:
    <ul>
        <li>Có 4 thành phố (đáp ứng yêu cầu tối thiểu)</li>
        <li>2 thành phố (Newark, New York City) bắt đầu bằng &#39;N&#39; (giống tên bang)</li>
        <li>matching_letter_count = 2</li>
    </ul>
    </li>
    <li><strong>California</strong> không được đưa vào kết quả vì:
    <ul>
        <li>Mặc dù có 4 thành phố (đáp ứng yêu cầu tối thiểu)</li>
        <li>Không có thành phố nào bắt đầu bằng &#39;C&#39; (không đáp ứng yêu cầu về chữ cái trùng khớp)</li>
    </ul>
    </li>

</ul>

<p><strong>Lưu ý:</strong></p>

<ul>
    <li>Kết quả được sắp xếp theo matching_letter_count theo thứ tự giảm dần</li>
    <li>Khi matching_letter_count bằng nhau (Texas và New York đều có 2), chúng được sắp xếp theo tên bang theo thứ tự alphabet</li>
    <li>Các thành phố trong mỗi hàng được sắp xếp theo thứ tự alphabet</li>
</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gộp nhóm + Lọc

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta phải liệt kê các thành phố theo từng bang, đếm số tên thành phố có cùng chữ cái đầu với tên bang, và giữ lại các bang có ít nhất ba thành phố cùng số lượng trùng khớp lớn hơn 0.
>
> Đây là các giá trị tổng hợp theo nhóm: phép nối đã sắp xếp, tổng boolean và phép đếm. Một cờ đánh dấu khớp cùng với $\textit{groupby}$ sẽ tính được tất cả trong một lượt.
>
> Sau khi lọc, chúng ta sắp xếp theo số lượng trùng khớp giảm dần rồi theo tên bang tăng dần, đồng thời loại bỏ cột đếm thành phố hỗ trợ.

<!-- thinking:end -->

Chúng ta có thể nhóm bảng `cities` theo trường `state`, sau đó lọc từng nhóm để chỉ giữ lại các nhóm đáp ứng những điều kiện đã nêu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    state,
    GROUP_CONCAT(city ORDER BY city SEPARATOR ', ') AS cities,
    COUNT(
        CASE
            WHEN LEFT(city, 1) = LEFT(state, 1) THEN 1
        END
    ) AS matching_letter_count
FROM cities
GROUP BY 1
HAVING COUNT(city) >= 3 AND matching_letter_count > 0
ORDER BY 3 DESC, 1;
```

#### Pandas

```python
import pandas as pd


def state_city_analysis(cities: pd.DataFrame) -> pd.DataFrame:
    cities["matching_letter"] = cities["city"].str[0] == cities["state"].str[0]

    result = (
        cities.groupby("state")
        .agg(
            cities=("city", lambda x: ", ".join(sorted(x))),
            matching_letter_count=("matching_letter", "sum"),
            city_count=("city", "count"),
        )
        .reset_index()
    )

    result = result[(result["city_count"] >= 3) & (result["matching_letter_count"] > 0)]

    result = result.sort_values(
        by=["matching_letter_count", "state"], ascending=[False, True]
    )

    result = result.drop(columns=["city_count"])

    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
