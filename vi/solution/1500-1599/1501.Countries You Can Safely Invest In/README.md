---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1501. Countries You Can Safely Invest In 🔒](https://leetcode.com/problems/countries-you-can-safely-invest-in)

[Tài liệu tiếng Trung](/solution/1500-1599/1501.Countries%20You%20Can%20Safely%20Invest%20In/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng <code>Person</code>:</p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| id             | int     |
| name           | varchar |
| phone_number   | varchar |
+----------------+---------+
id là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng của bảng này chứa tên và số điện thoại của một người.
Số điện thoại có dạng &#39;xxx-yyyyyyy&#39;, trong đó xxx là mã quốc gia (3 ký tự) và yyyyyyy là số điện thoại (7 ký tự), với x và y là các chữ số. Cả hai phần đều có thể chứa các số 0 ở đầu.
</pre>

<p>&nbsp;</p>

<p>Bảng <code>Country</code>:</p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| name           | varchar |
| country_code   | varchar |
+----------------+---------+
country_code là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng của bảng này chứa tên quốc gia và mã của quốc gia đó. country_code có dạng &#39;xxx&#39;, trong đó x là các chữ số.
</pre>

<p>&nbsp;</p>

<p>Bảng <code>Calls</code>:</p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| caller_id   | int  |
| callee_id   | int  |
| duration    | int  |
+-------------+------+
Bảng này có thể chứa các hàng trùng lặp.
Mỗi hàng của bảng này chứa id của người gọi, id của người nhận cuộc gọi và thời lượng cuộc gọi tính bằng phút. caller_id != callee_id
</pre>

<p>&nbsp;</p>

<p>Một công ty viễn thông muốn đầu tư vào các quốc gia mới. Công ty dự định đầu tư vào những quốc gia có thời lượng cuộc gọi trung bình trong quốc gia đó lớn hơn nghiêm ngặt so với thời lượng cuộc gọi trung bình trên toàn cầu.</p>

<p>Hãy viết lời giải để tìm các quốc gia mà công ty có thể đầu tư.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Person:
+----+----------+--------------+
| id | name     | phone_number |
+----+----------+--------------+
| 3  | Jonathan | 051-1234567  |
| 12 | Elvis    | 051-7654321  |
| 1  | Moncef   | 212-1234567  |
| 2  | Maroua   | 212-6523651  |
| 7  | Meir     | 972-1234567  |
| 9  | Rachel   | 972-0011100  |
+----+----------+--------------+
Bảng Country:
+----------+--------------+
| name     | country_code |
+----------+--------------+
| Peru     | 051          |
| Israel   | 972          |
| Morocco  | 212          |
| Germany  | 049          |
| Ethiopia | 251          |
+----------+--------------+
Bảng Calls:
+-----------+-----------+----------+
| caller_id | callee_id | duration |
+-----------+-----------+----------+
| 1         | 9         | 33       |
| 2         | 9         | 4        |
| 1         | 2         | 59       |
| 3         | 12        | 102      |
| 3         | 12        | 330      |
| 12        | 3         | 5        |
| 7         | 9         | 13       |
| 7         | 1         | 3        |
| 9         | 7         | 1        |
| 1         | 7         | 7        |
+-----------+-----------+----------+
<strong>Đầu ra:</strong>
+----------+
| country  |
+----------+
| Peru     |
+----------+
<strong>Giải thích:</strong>
Thời lượng cuộc gọi trung bình của Peru là (102 + 102 + 330 + 330 + 5 + 5) / 6 = 145.666667
Thời lượng cuộc gọi trung bình của Israel là (33 + 4 + 13 + 13 + 3 + 1 + 1 + 7) / 8 = 9.37500
Thời lượng cuộc gọi trung bình của Morocco là (33 + 4 + 59 + 59 + 3 + 7) / 6 = 27.5000
Thời lượng cuộc gọi trung bình trên toàn cầu = (2 * (33 + 4 + 59 + 102 + 330 + 5 + 13 + 3 + 1 + 7)) / 20 = 55.70000
Vì Peru là quốc gia duy nhất có thời lượng cuộc gọi trung bình lớn hơn mức trung bình trên toàn cầu, đây là quốc gia duy nhất được khuyến nghị.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + Group By + Subquery

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm những quốc gia có thời lượng cuộc gọi trung bình lớn hơn nghiêm ngặt so với mức trung bình trên toàn cầu. Bảng Calls chỉ lưu hai id của người dùng, còn quốc gia được xác định bởi ba chữ số đầu của số điện thoại, vì vậy cần liên kết các bảng Person, Calls và Country trước khi gom nhóm theo quốc gia.
>
> Hãy nối $Person$ với $Calls$ khi người đó là người gọi hoặc người nhận cuộc gọi, sau đó đối chiếu $Country$ bằng tiền tố của số điện thoại. Gom nhóm theo quốc gia sẽ cho thời lượng trung bình của từng quốc gia, rồi ta so sánh với mức trung bình toàn cầu của $Calls$. Một subquery cung cấp mức trung bình toàn cầu; query bên ngoài giữ lại các quốc gia thỏa mãn.

<!-- thinking:end -->

Ta có thể dùng equi-join để nối bảng `Person` và bảng `Calls` với điều kiện `Person.id = Calls.caller_id` hoặc `Person.id = Calls.callee_id`, sau đó nối kết quả với bảng `Country` bằng điều kiện `left(phone_number, 3) = country_code`. Tiếp theo, ta gom nhóm theo quốc gia và tính thời lượng cuộc gọi trung bình cho từng quốc gia. Cuối cùng, ta dùng một subquery để tìm các quốc gia có thời lượng cuộc gọi trung bình lớn hơn thời lượng cuộc gọi trung bình trên toàn cầu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT country
FROM
    (
        SELECT c.name AS country, AVG(duration) AS duration
        FROM
            Person
            JOIN Calls ON id IN(caller_id, callee_id)
            JOIN Country AS c ON LEFT(phone_number, 3) = country_code
        GROUP BY 1
    ) AS t
WHERE duration > (SELECT AVG(duration) FROM Calls);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Subquery lồng nhau trong Lời giải 1 đã tính thời lượng trung bình theo quốc gia và so sánh với mức trung bình toàn cầu, nhưng quan hệ trung gian còn được bọc thêm một lần nữa. Một CTE materialize các giá trị trung bình theo quốc gia thành $T$, nhờ đó bộ lọc bên ngoài dễ đọc hơn trong khi các phép nối và gom nhóm vẫn giữ nguyên.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT c.name AS country, AVG(duration) AS duration
        FROM
            Person
            JOIN Calls ON id IN(caller_id, callee_id)
            JOIN Country AS c ON LEFT(phone_number, 3) = country_code
        GROUP BY 1
    )
SELECT country
FROM T
WHERE duration > (SELECT AVG(duration) FROM Calls);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
