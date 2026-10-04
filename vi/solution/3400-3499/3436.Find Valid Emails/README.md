---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3436. Find Valid Emails](https://leetcode.com/problems/find-valid-emails)

[中文文档](/solution/3400-3499/3436.Find%20Valid%20Emails/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Users</code></p>

<pre>
+-----------------+---------+
| Column Name     | Type    |
+-----------------+---------+
| user_id         | int     |
| email           | varchar |
+-----------------+---------+
(user_id) là khóa duy nhất của bảng này.
Mỗi hàng chứa ID duy nhất và địa chỉ email của một người dùng.
</pre>

<p>Viết lời giải để tìm tất cả <strong>địa chỉ email hợp lệ</strong>. Một địa chỉ email hợp lệ phải đáp ứng các tiêu chí sau:</p>

<ul>
    <li>Chứa chính xác một ký hiệu <code>@</code>.</li>
    <li>Kết thúc bằng <code>.com</code>.</li>
    <li>Phần trước ký hiệu <code>@</code> chỉ chứa các ký tự <strong>chữ và số</strong> cùng <strong>gạch dưới</strong>.</li>
    <li>Phần sau ký hiệu <code>@</code> và trước <code>.com</code> chứa một tên miền <strong>chỉ gồm các chữ cái</strong>.</li>
</ul>

<p>Trả về<em> bảng kết quả được sắp xếp theo</em> <code>user_id</code> <em>theo</em> <strong>thứ tự </strong><em>tăng dần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Users:</p>

<pre class="example-io">
+---------+---------------------+
| user_id | email               |
+---------+---------------------+
| 1       | alice@example.com   |
| 2       | bob_at_example.com  |
| 3       | charlie@example.net |
| 4       | david@domain.com    |
| 5       | eve@invalid         |
+---------+---------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+-------------------+
| user_id | email             |
+---------+-------------------+
| 1       | alice@example.com |
| 4       | david@domain.com  |
+---------+-------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>alice@example.com</strong> hợp lệ vì chứa một <code>@</code>, alice chỉ gồm chữ và số, còn example.com bắt đầu bằng một chữ cái và kết thúc bằng .com.</li>
    <li><strong>bob_at_example.com</strong> không hợp lệ vì chứa gạch dưới thay cho <code>@</code>.</li>
    <li><strong>charlie@example.net</strong> không hợp lệ vì tên miền không kết thúc bằng <code>.com</code>.</li>
    <li><strong>david@domain.com</strong> hợp lệ vì đáp ứng tất cả tiêu chí.</li>
    <li><strong>eve@invalid</strong> không hợp lệ vì tên miền không kết thúc bằng <code>.com</code>.</li>
</ul>

<p>Bảng kết quả được sắp xếp theo user_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Regular Expression

<!-- thinking:start -->

> **Tư duy**
>
> Cấu trúc email trong đề bài đã được cố định: phần local chỉ gồm chữ, số và gạch dưới; domain bắt đầu bằng một chữ cái và kết thúc bằng `.com`. Việc tự tách chuỗi có thể bỏ sót các trường hợp biên.
>
> Một regular expression có anchor ở cả hai đầu sẽ kiểm tra đồng thời cả hai đầu chuỗi và các nhóm ký tự.
>
> Chúng ta giữ lại các hàng khớp với `^[A-Za-z0-9_]+@[A-Za-z][A-Za-z0-9]*\.com$` và sắp xếp theo $\textit{user\_id}$.

<!-- thinking:end -->

Chúng ta có thể dùng regular expression với `REGEXP` để khớp các địa chỉ email hợp lệ.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(1)$. Ở đây, $n$ là độ dài của chuỗi đầu vào.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT user_id, email
FROM Users
WHERE email REGEXP '^[A-Za-z0-9_]+@[A-Za-z][A-Za-z0-9]*\\.com$'
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def find_valid_emails(users: pd.DataFrame) -> pd.DataFrame:
    email_pattern = r"^[A-Za-z0-9_]+@[A-Za-z][A-Za-z0-9]*\.com$"
    valid_emails = users[users["email"].str.match(email_pattern)]
    valid_emails = valid_emails.sort_values(by="user_id")
    return valid_emails
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
