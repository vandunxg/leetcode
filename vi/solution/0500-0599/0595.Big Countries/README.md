---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [595. Big Countries](https://leetcode.com/problems/big-countries)

[中文文档](/solution/0500-0599/0595.Big%20Countries/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>World</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |
+-------------+---------+
name là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng cung cấp thông tin về tên quốc gia, châu lục mà quốc gia đó thuộc về, diện tích, dân số và GDP.
</pre>

<p>&nbsp;</p>

<p>Một quốc gia được xem là <strong>lớn</strong> nếu:</p>

<ul>
	<li>có diện tích ít nhất ba triệu (tức <code>3000000 km<sup>2</sup></code>), hoặc</li>
	<li>có dân số ít nhất hai mươi lăm triệu (tức <code>25000000</code>).</li>
</ul>

<p>Viết lời giải để tìm tên, dân số và diện tích của các <strong>quốc gia lớn</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng World:
+-------------+-----------+---------+------------+--------------+
| name        | continent | area    | population | gdp          |
+-------------+-----------+---------+------------+--------------+
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |
| Andorra     | Europe    | 468     | 78115      | 3712000000   |
| Angola      | Africa    | 1246700 | 20609294   | 100990000000 |
+-------------+-----------+---------+------------+--------------+
<strong>Đầu ra:</strong> 
+-------------+------------+---------+
| name        | population | area    |
+-------------+------------+---------+
| Afghanistan | 25500100   | 652230  |
| Algeria     | 37100000   | 2381741 |
+-------------+------------+---------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Quốc gia lớn phải đạt ngưỡng về diện tích hoặc dân số. Chỉ cần một điều kiện lọc.
>
> `WHERE area >= 3000000 OR population >= 25000000` giữ lại hàng thỏa mãn một trong hai điều kiện. Dùng OR để không bỏ sót các hàng chỉ thỏa mãn một điều kiện.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng `OR`. Ta cũng có thể dùng hai truy vấn riêng rồi hợp kết quả; cách này loại bỏ các hàng trùng lặp.
>
> Nếu `OR` khiến query không tận dụng được index, hai lần quét theo khoảng kết hợp bằng `UNION` có thể ổn định hơn. Tập hàng thu được giống Lời giải 1.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT name, population, area
FROM World
WHERE area >= 3000000
UNION
SELECT name, population, area
FROM World
WHERE population >= 25000000;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
