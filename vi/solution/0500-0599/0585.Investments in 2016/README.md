---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [585. Investments in 2016](https://leetcode.com/problems/investments-in-2016)

[中文文档](/solution/0500-0599/0585.Investments%20in%202016/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Insurance</code></p>

<pre>
+-------------+-------+
| Tên cột    | Kiểu dữ liệu |
+-------------+-------+
| pid         | int   |
| tiv_2015    | float |
| tiv_2016    | float |
| lat         | float |
| lon         | float |
+-------------+-------+
pid là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng chứa thông tin về một hợp đồng bảo hiểm, gồm:
pid là mã hợp đồng bảo hiểm của người mua.
tiv_2015 là tổng giá trị đầu tư trong năm 2015, còn tiv_2016 là tổng giá trị đầu tư trong năm 2016.
lat là vĩ độ thành phố nơi người mua cư trú. Đảm bảo lat không phải NULL.
lon là kinh độ thành phố nơi người mua cư trú. Đảm bảo lon không phải NULL.
</pre>

<p>&nbsp;</p>

<p>Hãy viết truy vấn tính tổng giá trị đầu tư năm 2016 <code>tiv_2016</code> của những người mua thỏa mãn cả hai điều kiện:</p>

<ul>
	<li>có giá trị <code>tiv_2015</code> trùng với ít nhất một người mua khác, và</li>
	<li>không ở cùng thành phố với bất kỳ người mua nào khác (tức là các cặp thuộc tính (<code>lat, lon</code>) phải duy nhất).</li>
</ul>

<p>Làm tròn <code>tiv_2016</code> đến <strong>hai chữ số thập phân</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Insurance:
+-----+----------+----------+-----+-----+
| pid | tiv_2015 | tiv_2016 | lat | lon |
+-----+----------+----------+-----+-----+
| 1   | 10       | 5        | 10  | 10  |
| 2   | 20       | 20       | 20  | 20  |
| 3   | 10       | 30       | 20  | 20  |
| 4   | 10       | 40       | 40  | 40  |
+-----+----------+----------+-----+-----+
<strong>Đầu ra:</strong> 
+----------+
| tiv_2016 |
+----------+
| 45.00    |
+----------+
<strong>Giải thích:</strong> 
Bản ghi đầu tiên trong bảng, cũng như bản ghi cuối cùng, thỏa mãn cả hai điều kiện.
Giá trị tiv_2015 bằng 10 trùng với giá trị của bản ghi thứ ba và thứ tư, còn vị trí của bản ghi này là duy nhất.

Bản ghi thứ hai không thỏa mãn điều kiện nào. tiv_2015 của nó không trùng với người mua nào khác, còn vị trí lại giống bản ghi thứ ba, khiến bản ghi thứ ba cũng không đạt.
Vì vậy, kết quả là tổng tiv_2016 của bản ghi đầu tiên và bản ghi cuối cùng, bằng 45.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một hợp đồng năm 2016 được tính nếu giá trị đầu tư năm 2015 của nó trùng với hợp đồng khác và vị trí của nó là duy nhất. Quét lồng nhau cho từng hàng sẽ có độ phức tạp bậc hai.
>
> Dùng window function để đếm theo `tiv_2015` và theo `(lat, lon)`. Giữ các hàng có $cnt1>1$ và $cnt2=1$, tính tổng `tiv_2016` rồi làm tròn. Một lượt window function thay thế các subquery tương quan.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            tiv_2016,
            COUNT(1) OVER (PARTITION BY tiv_2015) AS cnt1,
            COUNT(1) OVER (PARTITION BY lat, lon) AS cnt2
        FROM Insurance
    )
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM T
WHERE cnt1 > 1 AND cnt2 = 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
