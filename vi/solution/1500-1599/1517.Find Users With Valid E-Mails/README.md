---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1517. Find Users With Valid E-Mails](https://leetcode.com/problems/find-users-with-valid-e-mails)

[中文文档](/solution/1500-1599/1517.Find%20Users%20With%20Valid%20E-Mails/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Users</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
| mail          | varchar |
+---------------+---------+
user_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Bảng này chứa thông tin về người dùng đã đăng ký trên một website. Một số email không hợp lệ.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để tìm những người dùng có <strong>email hợp lệ</strong>.</p>

<p>Một email hợp lệ có tên tiền tố và domain thỏa các điều kiện:</p>

<ul>
	<li><strong>Tên tiền tố</strong> là chuỗi có thể chứa chữ cái (hoa hoặc thường), chữ số, dấu gạch dưới <code>&#39;_&#39;</code>, dấu chấm <code>&#39;.&#39;</code> và/hoặc dấu gạch ngang <code>&#39;-&#39;</code>. Tên tiền tố <strong>phải</strong> bắt đầu bằng chữ cái.</li>
	<li><strong>Domain</strong> phải chính xác là <code>&#39;@leetcode.com&#39;</code> viết thường.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được thể hiện trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Users table:
+---------+-----------+-------------------------+
| user_id | name      | mail                    |
+---------+-----------+-------------------------+
| 1       | Winston   | winston@leetcode.com    |
| 2       | Jonathan  | jonathanisgreat         |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |
| 5       | Marwan    | quarz#2020@leetcode.com |
| 6       | David     | david69@gmail.com       |
| 7       | Shapiro   | .shapo@leetcode.com     |
+---------+-----------+-------------------------+
<strong>Output:</strong> 
+---------+-----------+-------------------------+
| user_id | name      | mail                    |
+---------+-----------+-------------------------+
| 1       | Winston   | winston@leetcode.com    |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |
+---------+-----------+-------------------------+
<strong>Explanation:</strong> 
Email của người dùng 2 không có domain.
Email của người dùng 5 chứa ký hiệu # không được phép.
Email của người dùng 6 không có domain leetcode.
Email của người dùng 7 bắt đầu bằng dấu chấm.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So khớp bằng mẫu REGEXP

<!-- thinking:start -->

> **Tư duy**
>
> Địa chỉ hợp lệ phải bắt đầu bằng chữ cái, tiếp theo là chữ cái, chữ số, dấu gạch dưới, dấu chấm hoặc dấu gạch ngang, và kết thúc bằng $\texttt{@leetcode.com}$. Việc tự kiểm tra từng ký tự tạo nhiều nhánh và dễ bỏ sót trường hợp biên, trong khi quy tắc này là một regular language.
>
> The pattern $\texttt{^[A-Za-z][A-Za-z0-9_.-]*@leetcode\\.com$}$ matches the whole string. In SQL a case-sensitive suffix check guards the domain; the Pandas path applies the same full-string match to the $mail$ column.

<!-- thinking:end -->

Ta có thể dùng regular expression để khớp định dạng email hợp lệ. Biểu thức đảm bảo phần username tuân theo các quy tắc yêu cầu và domain cố định là `@leetcode.com`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT *
FROM Users
WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$' AND BINARY mail LIKE '%@leetcode.com';
```

#### Pandas

```python
import pandas as pd


def valid_emails(users: pd.DataFrame) -> pd.DataFrame:
    pattern = r"^[A-Za-z][A-Za-z0-9_.-]*@leetcode\.com$"
    mask = users["mail"].str.match(pattern, flags=0, na=False)
    return users.loc[mask, ["user_id", "name", "mail"]]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
