---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1907. Count Salary Categories](https://leetcode.com/problems/count-salary-categories)

[中文文档](/solution/1900-1999/1907.Count%20Salary%20Categories/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Accounts</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| income      | int  |
+-------------+------+
account_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về thu nhập hàng tháng của một tài khoản ngân hàng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tính số tài khoản ngân hàng thuộc mỗi nhóm lương. Các nhóm lương là:</p>

<ul>
	<li><code>&quot;Low Salary&quot;</code>: Tất cả mức lương <strong>nhỏ hơn nghiêm ngặt</strong> <code>$20000</code>.</li>
	<li><code>&quot;Average Salary&quot;</code>: Tất cả mức lương trong khoảng <strong>bao gồm hai đầu</strong> <code>[$20000, $50000]</code>.</li>
	<li><code>&quot;High Salary&quot;</code>: Tất cả mức lương <strong>lớn hơn nghiêm ngặt</strong> <code>$50000</code>.</li>
</ul>

<p>Bảng kết quả <strong>phải</strong> chứa cả ba nhóm. Nếu một nhóm không có tài khoản nào, hãy trả về <code>0</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Accounts:
+------------+--------+
| account_id | income |
+------------+--------+
| 3          | 108939 |
| 2          | 12747  |
| 8          | 87709  |
| 6          | 91796  |
+------------+--------+
<strong>Đầu ra:</strong>
+----------------+----------------+
| category       | accounts_count |
+----------------+----------------+
| Low Salary     | 1              |
| Average Salary | 0              |
| High Salary    | 3              |
+----------------+----------------+
<strong>Giải thích:</strong>
Low Salary: Tài khoản 2.
Average Salary: Không có tài khoản nào.
High Salary: Các tài khoản 3, 6 và 8.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng tạm + Gom nhóm + Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Một câu lệnh $\texttt{GROUP BY}$ đơn giản trên nhóm thu nhập sẽ loại bỏ nhóm không có tài khoản, trong khi kết quả luôn phải chứa đủ ba hàng với giá trị 0.
>
> Vì vậy, ta tạo một bảng ba hàng chứa các nhóm, gom nhóm bảng $\texttt{Accounts}$ bằng $\texttt{CASE}$, rồi left join để các nhóm bị thiếu nhận giá trị $0$ thông qua $\texttt{IFNULL}$.

<!-- thinking:end -->

Trước hết, ta có thể tạo một bảng tạm chứa tất cả các nhóm lương, sau đó đếm số tài khoản ngân hàng trong từng nhóm. Cuối cùng, ta dùng left join để nối bảng tạm với bảng kết quả nhằm đảm bảo bảng kết quả chứa đủ mọi nhóm lương.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT 'Low Salary' AS category
        UNION
        SELECT 'Average Salary'
        UNION
        SELECT 'High Salary'
    ),
    T AS (
        SELECT
            CASE
                WHEN income < 20000 THEN "Low Salary"
                WHEN income > 50000 THEN 'High Salary'
                ELSE 'Average Salary'
            END AS category,
            COUNT(1) AS accounts_count
        FROM Accounts
        GROUP BY 1
    )
SELECT category, IFNULL(accounts_count, 0) AS accounts_count
FROM
    S
    LEFT JOIN T USING (category);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lọc + Gộp

<!-- thinking:start -->

> **Tư duy**
>
> Cách dùng bảng tạm và phép nối quét dữ liệu hai lần. Ba nhóm không giao nhau, nên ba phép $\texttt{SUM}$ có điều kiện, gộp thành ba hàng, cũng tạo ra các giá trị 0 tương tự với câu truy vấn ngắn hơn.

<!-- thinking:end -->

Ta có thể lọc số tài khoản ngân hàng thuộc từng nhóm lương riêng biệt rồi gộp các kết quả. Ở đây, ta dùng `UNION` để gộp kết quả.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT 'Low Salary' AS category, IFNULL(SUM(income < 20000), 0) AS accounts_count FROM Accounts
UNION
SELECT
    'Average Salary' AS category,
    IFNULL(SUM(income BETWEEN 20000 AND 50000), 0) AS accounts_count
FROM Accounts
UNION
SELECT 'High Salary' AS category, IFNULL(SUM(income > 50000), 0) AS accounts_count FROM Accounts;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
