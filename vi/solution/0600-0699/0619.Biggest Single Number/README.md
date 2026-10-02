---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [619. Biggest Single Number](https://leetcode.com/problems/biggest-single-number)

[中文文档](/solution/0600-0699/0619.Biggest%20Single%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>MyNumbers</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| num         | int  |
+-------------+------+
Bảng này có thể chứa các giá trị trùng lặp (nói cách khác, bảng không có khóa chính trong SQL).
Mỗi hàng của bảng chứa một số nguyên.
</pre>

<p>&nbsp;</p>

<p><strong>Số xuất hiện một lần</strong> là số chỉ xuất hiện duy nhất một lần trong bảng <code>MyNumbers</code>.</p>

<p>Hãy tìm <strong>số xuất hiện một lần</strong> lớn nhất. Nếu không có số nào như vậy, trả về <code>null</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>
<ptable> </ptable>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng MyNumbers:
+-----+
| num |
+-----+
| 8   |
| 8   |
| 3   |
| 3   |
| 1   |
| 4   |
| 5   |
| 6   |
+-----+
<strong>Đầu ra:</strong> 
+-----+
| num |
+-----+
| 6   |
+-----+
<strong>Giải thích:</strong> Các số chỉ xuất hiện một lần là 1, 4, 5 và 6.
Vì 6 là số xuất hiện một lần lớn nhất, ta trả về số này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng MyNumbers:
+-----+
| num |
+-----+
| 8   |
| 8   |
| 7   |
| 7   |
| 3   |
| 3   |
| 3   |
+-----+
<strong>Đầu ra:</strong> 
+------+
| num  |
+------+
| null |
+------+
<strong>Giải thích:</strong> Bảng đầu vào không có số nào chỉ xuất hiện một lần nên ta trả về null.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By và Subquery

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số lớn nhất chỉ xuất hiện đúng một lần. Group By giúp đếm tần suất xuất hiện.
>
> `HAVING COUNT=1` giữ lại các số xuất hiện một lần; `MAX` ở truy vấn bên ngoài trả về số lớn nhất hoặc `NULL` nếu không có số nào.

<!-- thinking:end -->

Trước tiên, ta có thể nhóm bảng `MyNumbers` theo `num` và đếm số lần xuất hiện của mỗi số. Sau đó, dùng subquery để tìm số lớn nhất trong các số chỉ xuất hiện một lần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT MAX(num) AS num
FROM
    (
        SELECT num
        FROM MyNumbers
        GROUP BY 1
        HAVING COUNT(1) = 1
    ) AS t;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Group By và biểu thức `CASE`

<!-- thinking:start -->

> **Tư duy**
>
> Có thể bỏ subquery `MAX` bổ sung: sau khi group, dùng `CASE WHEN COUNT=1 THEN num`, rồi sắp xếp giảm dần và lấy một hàng.

<!-- thinking:end -->

Tương tự lời giải 1, trước tiên ta nhóm bảng `MyNumbers` theo `num` và đếm số lần xuất hiện của mỗi số. Sau đó, dùng biểu thức `CASE` để chọn các số chỉ xuất hiện một lần, sắp xếp chúng theo thứ tự giảm dần rồi lấy số đầu tiên.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    CASE
        WHEN COUNT(1) = 1 THEN num
        ELSE NULL
    END AS num
FROM MyNumbers
GROUP BY num
ORDER BY 1 DESC
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
