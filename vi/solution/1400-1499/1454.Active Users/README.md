---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1454. Active Users 🔒](https://leetcode.com/problems/active-users)

[中文文档](/solution/1400-1499/1454.Active%20Users/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Accounts</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
id là khóa chính (cột có các giá trị không trùng lặp) của bảng này.
Bảng này chứa id tài khoản và tên người dùng của mỗi tài khoản.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Logins</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| login_date    | date    |
+---------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Bảng này chứa id tài khoản của người dùng đã đăng nhập và ngày đăng nhập. Một người dùng có thể đăng nhập nhiều lần trong cùng một ngày.
</pre>

<p>&nbsp;</p>

<p><strong>Người dùng hoạt động</strong> là những người đã đăng nhập vào tài khoản trong ít nhất năm ngày liên tiếp.</p>

<p>Hãy viết lời giải để tìm id và tên của <strong>người dùng hoạt động</strong>.</p>

<p>Trả về bảng kết quả được <strong>sắp xếp</strong> theo <code>id</code>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Accounts:
+----+----------+
| id | name     |
+----+----------+
| 1  | Winston  |
| 7  | Jonathan |
+----+----------+
Bảng Logins:
+----+------------+
| id | login_date |
+----+------------+
| 7  | 2020-05-30 |
| 1  | 2020-05-30 |
| 7  | 2020-05-31 |
| 7  | 2020-06-01 |
| 7  | 2020-06-02 |
| 7  | 2020-06-02 |
| 7  | 2020-06-03 |
| 1  | 2020-06-07 |
| 7  | 2020-06-10 |
+----+------------+
<strong>Đầu ra:</strong>
+----+----------+
| id | name     |
+----+----------+
| 7  | Jonathan |
+----+----------+
<strong>Giải thích:</strong>
Người dùng Winston với id = 1 chỉ đăng nhập 2 lần vào 2 ngày khác nhau, nên Winston không phải là người dùng hoạt động.
Người dùng Jonathan với id = 7 đã đăng nhập 7 lần trong 6 ngày khác nhau, trong đó có 5 ngày liên tiếp, nên Jonathan là người dùng hoạt động.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể viết lời giải tổng quát nếu người dùng hoạt động là những người đã đăng nhập vào tài khoản trong <code>n</code> ngày liên tiếp trở lên không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng Window Function

<!-- thinking:start -->

> **Tư duy**
>
> Một người dùng hoạt động đã đăng nhập trong ít nhất năm ngày liên tiếp; các lần đăng nhập trùng trong cùng ngày phải được loại bỏ. Lấy ngày đăng nhập trừ `ROW_NUMBER()` (theo từng người dùng và theo ngày) sẽ cho một giá trị $g$ không đổi trong một chuỗi liên tiếp.
>
> Nhóm theo $(id,g)$ và giữ lại những người dùng có ít nhất năm hàng, sau đó xuất các id và tên không trùng lặp.

<!-- thinking:end -->

Đầu tiên, chúng ta join bảng `Logins` với bảng `Accounts` và loại bỏ các bản ghi trùng lặp để thu được bảng tạm `T`.

Sau đó, chúng ta sử dụng hàm cửa sổ `ROW_NUMBER()` để tính ngày đăng nhập cơ sở `g` cho mỗi người dùng `id`. Nếu một người dùng đăng nhập trong 5 ngày liên tiếp, các giá trị `g` của họ sẽ giống nhau.

Cuối cùng, chúng ta nhóm theo `id` và `g` để đếm số lần đăng nhập của mỗi người dùng. Nếu số lần đăng nhập lớn hơn hoặc bằng 5, người dùng đó được xem là người dùng hoạt động.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT DISTINCT *
        FROM
            Logins
            JOIN Accounts USING (id)
    ),
    P AS (
        SELECT
            *,
            DATE_SUB(
                login_date,
                INTERVAL ROW_NUMBER() OVER (
                    PARTITION BY id
                    ORDER BY login_date
                ) DAY
            ) g
        FROM T
    )
SELECT DISTINCT id, name
FROM P
GROUP BY id, g
HAVING COUNT(*) >= 5
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
