---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3059. Find All Unique Email Domains 🔒](https://leetcode.com/problems/find-all-unique-email-domains)

[中文文档](/solution/3000-3099/3059.Find%20All%20Unique%20Email%20Domains/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Emails</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này chứa một email. Các email sẽ không chứa chữ cái viết hoa.
</pre>

<p>Viết lời giải để tìm tất cả <strong>domain email duy nhất</strong> và đếm số <strong>người dùng</strong> tương ứng với mỗi domain. <strong>Chỉ xét</strong> những domain <strong>kết thúc</strong> bằng <strong>.com</strong>.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo domain email theo thứ tự </em><strong>tăng dần</strong><em>.</em></p>

<p>Định dạng của bảng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Emails:
+-----+-----------------------+
| id  | email                 |
+-----+-----------------------+
| 336 | hwkiy@test.edu        |
| 489 | adcmaf@outlook.com    |
| 449 | vrzmwyum@yahoo.com    |
| 95  | tof@test.edu          |
| 320 | jxhbagkpm@example.org |
| 411 | zxcf@outlook.com      |
+----+------------------------+
<strong>Đầu ra:</strong>
+--------------+-------+
| email_domain | count |
+--------------+-------+
| outlook.com  | 2     |
| yahoo.com    | 1     |
+--------------+-------+
<strong>Giải thích:</strong>
- Các domain hợp lệ kết thúc bằng &quot;.com&quot; chỉ có &quot;outlook.com&quot; và &quot;yahoo.com&quot;, với số lượng tương ứng là 2 và 1.
Bảng kết quả được sắp xếp theo email_domains theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng hàm `SUBSTRING_INDEX` + Thống kê theo nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm domain của các địa chỉ kết thúc bằng `.com`. Domain là phần nằm sau `@`.
>
> Tách domain, sau đó chỉ giữ lại `.com` để loại bỏ các host cục bộ. Nhóm và đếm theo domain.
>
> Code lấy phần tử cuối cùng sau khi tách, lọc bằng `contains('.com')`, rồi nhóm các kết quả.

<!-- thinking:end -->

Đầu tiên, ta lọc ra tất cả email kết thúc bằng `.com`, sau đó sử dụng hàm `SUBSTRING_INDEX` để trích xuất domain của email. Cuối cùng, ta dùng `GROUP BY` để đếm số email của mỗi domain.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT SUBSTRING_INDEX(email, '@', -1) AS email_domain, COUNT(1) AS count
FROM Emails
WHERE email LIKE '%.com'
GROUP BY 1
ORDER BY 1;
```

#### Python3

```python
import pandas as pd


def find_unique_email_domains(emails: pd.DataFrame) -> pd.DataFrame:
    emails["email_domain"] = emails["email"].str.split("@").str[-1]
    emails = emails[emails["email"].str.contains(".com")]
    return (
        emails.groupby("email_domain")
        .size()
        .reset_index(name="count")
        .sort_values(by="email_domain")
    )
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
