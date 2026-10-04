---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3118. Friday Purchase III 🔒](https://leetcode.com/problems/friday-purchase-iii)

[中文文档](/solution/3100-3199/3118.Friday%20Purchase%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Purchases</code></p>

<pre>
+---------------+------+
| Column Name   | Type |
+---------------+------+
| user_id       | int  |
| purchase_date | date |
| amount_spend  | int  |
+---------------+------+
(user_id, purchase_date, amount_spend) is the primary key (combination of columns with unique values) for this table.
purchase_date will range from November 1, 2023, to November 30, 2023, inclusive of both dates.
Each row contains user_id, purchase_date, and amount_spend.
</pre>

<p>Bảng: <code>Users</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| membership  | enum |
+-------------+------+
user_id is the primary key for this table.
membership is an ENUM (category) type of (&#39;Standard&#39;, &#39;Premium&#39;, &#39;VIP&#39;).
Each row of this table indicates the user_id, membership type.
</pre>

<p>Hãy viết lời giải để tính <strong>tổng chi tiêu</strong> của các thành viên <code>Premium</code> và <code>VIP</code> vào <strong>mỗi thứ Sáu của từng tuần</strong> trong tháng 11 năm 2023. Nếu <strong>không có giao dịch</strong> nào vào một <strong>ngày thứ Sáu cụ thể</strong> của thành viên <code>Premium</code> hoặc <code>VIP</code>, giá trị đó được xem là <code>0</code>.</p>

<p>Trả về <em>bảng kết quả</em>&nbsp;<em>được sắp xếp theo tuần trong tháng,&nbsp; và </em><code>membership</code><em> theo <strong>thứ tự tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Purchases:</p>

<pre class="example-io">
+---------+---------------+--------------+
| user_id | purchase_date | amount_spend |
+---------+---------------+--------------+
| 11      | 2023-11-03    | 1126         |
| 15      | 2023-11-10    | 7473         |
| 17      | 2023-11-17    | 2414         |
| 12      | 2023-11-24    | 9692         |
| 8       | 2023-11-24    | 5117         |
| 1       | 2023-11-24    | 5241         |
| 10      | 2023-11-22    | 8266         |
| 13      | 2023-11-21    | 12000        |
+---------+---------------+--------------+
</pre>

<p>Bảng Users:</p>

<pre class="example-io">
+---------+------------+
| user_id | membership |
+---------+------------+
| 11      | Premium    |
| 15      | VIP        |
| 17      | Standard   |
| 12      | VIP        |
| 8       | Premium    |
| 1       | VIP        |
| 10      | Standard   |
| 13      | Premium    |
+---------+------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------------+-------------+--------------+
| week_of_month | membership  | total_amount |
+---------------+-------------+--------------+
| 1             | Premium     | 1126         |
| 1             | VIP         | 0            |
| 2             | Premium     | 0            |
| 2             | VIP         | 7473         |
| 3             | Premium     | 0            |
| 3             | VIP         | 0            |
| 4             | Premium     | 5117         |
| 4             | VIP         | 14933        |
+---------------+-------------+--------------+
        </pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Trong tuần đầu tiên của tháng 11 năm 2023, có một giao dịch vào thứ Sáu, ngày 2023-11-03, do một thành viên Premium thực hiện với số tiền $1,126. Không có giao dịch nào được thực hiện bởi thành viên VIP trong ngày này, nên giá trị là 0.</li>
	<li>Trong tuần thứ hai của tháng 11 năm 2023, có một giao dịch vào thứ Sáu, ngày 2023-11-10, do một thành viên VIP thực hiện với số tiền $7,473. Vì thứ Sáu đó không có giao dịch nào của thành viên Premium, kết quả của thành viên Premium là 0.</li>
	<li>Tương tự, trong tuần thứ ba của tháng 11 năm 2023, không có giao dịch nào của thành viên Premium hoặc VIP vào thứ Sáu, ngày 2023-11-17, nên cả hai nhóm đều có giá trị 0 trong tuần này.</li>
	<li>Trong tuần thứ tư của tháng 11 năm 2023, có các giao dịch vào thứ Sáu, ngày 2023-11-24, gồm một giao dịch của thành viên Premium trị giá $5,117 and VIP member purchases totaling $14,933 ($9,692 from one and $5,241 từ một thành viên khác).</li>
</ul>

<p><strong>Lưu ý:</strong> Bảng kết quả được sắp xếp theo week_of_month và membership theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy + Join

<!-- thinking:start -->

> **Tư duy**
>
> Chi tiêu vào thứ Sáu phải được báo cáo cho mọi tổ hợp tuần trong tháng và membership, kể cả những tổ hợp không có dòng dữ liệu. Nếu chỉ quét Purchases, các nhóm rỗng sẽ bị bỏ qua.
>
> Vì các tuần là $1$ đến $4$ và membership chỉ gồm Premium và VIP, ta có thể dùng CTE đệ quy cùng `UNION` để tạo đầy đủ ma trận trước khi left join với các dòng thực tế vào thứ Sáu.
>
> Xây dựng `T`, `M` và `P` đã lọc, cross `T` với `M`, left-join `P`, rồi tính tổng `amount_spend` theo tuần và membership, thay các giá trị null bằng $0$.

<!-- thinking:end -->

Đầu tiên, chúng ta tạo một bảng đệ quy `T` chứa cột `week_of_month`, biểu thị tuần trong tháng. Sau đó, chúng ta tạo bảng `M` chứa cột `membership`, biểu thị loại thành viên, với các giá trị `'Premium'` và `'VIP'`.

Tiếp theo, chúng ta tạo bảng `P` chứa các cột `week_of_month`, `membership` và `amount_spend`, lọc số tiền mỗi thành viên đã chi vào các ngày thứ Sáu của từng tuần trong tháng. Cuối cùng, chúng ta join bảng `T` với `M`, sau đó left join với bảng `P`, rồi nhóm theo các cột `week_of_month` và `membership` để tính tổng chi tiêu của từng loại thành viên trong mỗi tuần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH RECURSIVE
    T AS (
        SELECT 1 AS week_of_month
        UNION
        SELECT week_of_month + 1
        FROM T
        WHERE week_of_month < 4
    ),
    M AS (
        SELECT 'Premium' AS membership
        UNION
        SELECT 'VIP'
    ),
    P AS (
        SELECT CEIL(DAYOFMONTH(purchase_date) / 7) AS week_of_month, membership, amount_spend
        FROM
            Purchases
            JOIN Users USING (user_id)
        WHERE DAYOFWEEK(purchase_date) = 6
    )
SELECT week_of_month, membership, IFNULL(SUM(amount_spend), 0) AS total_amount
FROM
    T
    JOIN M
    LEFT JOIN P USING (week_of_month, membership)
GROUP BY 1, 2
ORDER BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
