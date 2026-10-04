---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3198. Find Cities in Each State 🔒](https://leetcode.com/problems/find-cities-in-each-state)

[中文文档](/solution/3100-3199/3198.Find%20Cities%20in%20Each%20State/README.md)

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
(state, city) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này chứa tên bang và tên thành phố thuộc bang đó.
</pre>

<p>Viết lời giải để tìm <strong>tất cả thành phố trong mỗi bang</strong> và kết hợp chúng thành một chuỗi <strong>duy nhất, phân tách bằng dấu phẩy</strong>.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>state</code>&nbsp;<em>và</em> <code>city</code>&nbsp;<em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng cities:</p>

<pre class="example-io">
+-------------+---------------+
| state       | city          |
+-------------+---------------+
| California  | Los Angeles   |
| California  | San Francisco |
| California  | San Diego     |
| Texas       | Houston       |
| Texas       | Austin        |
| Texas       | Dallas        |
| New York    | New York City |
| New York    | Buffalo       |
| New York    | Rochester     |
+-------------+---------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-------------+---------------------------------------+
| state       | cities                                |
+-------------+---------------------------------------+
| California  | Los Angeles, San Diego, San Francisco |
| New York    | Buffalo, New York City, Rochester     |
| Texas       | Austin, Dallas, Houston               |
+-------------+---------------------------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>California:</strong> Tất cả thành phố (&quot;Los Angeles&quot;, &quot;San Diego&quot;, &quot;San Francisco&quot;) được liệt kê trong một chuỗi phân tách bằng dấu phẩy.</li>
    <li><strong>New York:</strong> Tất cả thành phố (&quot;Buffalo&quot;, &quot;New York City&quot;, &quot;Rochester&quot;) được liệt kê trong một chuỗi phân tách bằng dấu phẩy.</li>
    <li><strong>Texas:</strong> Tất cả thành phố (&quot;Austin&quot;, &quot;Dallas&quot;, &quot;Houston&quot;) được liệt kê trong một chuỗi phân tách bằng dấu phẩy.</li>
</ul>

<p><strong>Lưu ý:</strong> Bảng kết quả được sắp xếp theo tên bang theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Thành phố của mỗi bang phải được liệt kê theo thứ tự từ điển và nối bằng dấu phẩy cùng khoảng trắng. Đây là phép tổng hợp theo nhóm với bước sắp xếp bên trong.
>
> SQL sử dụng `GROUP_CONCAT(... ORDER BY city)`; pandas `groupby` và `join` một series thành phố đã `sorted`.
>
> Đặt tên các cột là $state$ và $cities$, mỗi bang một dòng.

<!-- thinking:end -->

Trước tiên, chúng ta có thể nhóm theo trường `state`, sau đó sắp xếp trường `city` trong mỗi nhóm, cuối cùng dùng hàm `GROUP_CONCAT` để nối tên các thành phố đã sắp xếp thành một chuỗi phân tách bằng dấu phẩy.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    state,
    GROUP_CONCAT(city ORDER BY city SEPARATOR ', ') cities
FROM cities
GROUP BY 1
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def find_cities(cities: pd.DataFrame) -> pd.DataFrame:
    result = (
        cities.groupby("state")["city"]
        .apply(lambda x: ", ".join(sorted(x)))
        .reset_index()
    )
    result.columns = ["state", "cities"]
    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
