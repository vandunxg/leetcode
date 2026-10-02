---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1127. User Purchase Platform 🔒](https://leetcode.com/problems/user-purchase-platform)

[中文文档](/solution/1100-1199/1127.User%20Purchase%20Platform/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Spending</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| user_id     | int     |
| spend_date  | date    |
| platform    | enum    | 
| amount      | int     |
+-------------+---------+
Bảng lưu lịch sử chi tiêu của người dùng mua hàng trên website thương mại điện tử có ứng dụng desktop và mobile.
(user_id, spend_date, platform) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Cột platform có kiểu ENUM (danh mục) với các giá trị (&#39;desktop&#39;, &#39;mobile&#39;).
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm tổng số người dùng và tổng số tiền đã chi cho từng ngày, theo ba nhóm: chỉ dùng mobile, chỉ dùng desktop và dùng cả mobile lẫn desktop.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng Spending:
+---------+------------+----------+--------+
| user_id | spend_date | platform | amount |
+---------+------------+----------+--------+
| 1       | 2019-07-01 | mobile   | 100    |
| 1       | 2019-07-01 | desktop  | 100    |
| 2       | 2019-07-01 | mobile   | 100    |
| 2       | 2019-07-02 | mobile   | 100    |
| 3       | 2019-07-01 | desktop  | 100    |
| 3       | 2019-07-02 | desktop  | 100    |
+---------+------------+----------+--------+
<strong>Đầu ra:</strong> 
+------------+----------+--------------+-------------+
| spend_date | platform | total_amount | total_users |
+------------+----------+--------------+-------------+
| 2019-07-01 | desktop  | 100          | 1           |
| 2019-07-01 | mobile   | 100          | 1           |
| 2019-07-01 | both     | 200          | 1           |
| 2019-07-02 | desktop  | 100          | 1           |
| 2019-07-02 | mobile   | 100          | 1           |
| 2019-07-02 | both     | 0            | 0           |
+------------+----------+--------------+-------------+ 
<strong>Giải thích:</strong> 
Ngày 2019-07-01, người dùng 1 mua hàng bằng <strong>cả</strong> desktop lẫn mobile, người dùng 2 chỉ dùng mobile và người dùng 3 chỉ dùng desktop.
Ngày 2019-07-02, người dùng 2 chỉ dùng mobile, người dùng 3 chỉ dùng desktop và không ai mua hàng bằng <strong>cả hai</strong> nền tảng.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cặp người dùng-ngày thuộc một trong ba nhóm: `desktop`, `mobile` hoặc `both`; mỗi ngày vẫn phải có đủ cả ba nhóm nền tảng. Gom dữ liệu theo người dùng và ngày: nếu chỉ có một nền tảng thì giữ nguyên, nếu không thì gán `both`.
>
> Tạo khung gồm mọi ngày kết hợp với ba nhãn nền tảng, rồi left join với dữ liệu đã gom nhóm và điền 0 để mỗi ngày có đủ ba hàng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT DISTINCT spend_date, 'desktop' AS platform FROM Spending
        UNION
        SELECT DISTINCT spend_date, 'mobile' FROM Spending
        UNION
        SELECT DISTINCT spend_date, 'both' FROM Spending
    ),
    T AS (
        SELECT
            user_id,
            spend_date,
            SUM(amount) AS amount,
            IF(COUNT(platform) = 1, platform, 'both') AS platform
        FROM Spending
        GROUP BY 1, 2
    )
SELECT
    p.*,
    IFNULL(SUM(amount), 0) AS total_amount,
    COUNT(t.user_id) AS total_users
FROM
    P AS p
    LEFT JOIN T AS t USING (spend_date, platform)
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
