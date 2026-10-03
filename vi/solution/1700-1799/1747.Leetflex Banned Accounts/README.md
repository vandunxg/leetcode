---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1747. Leetflex Banned Accounts 🔒](https://leetcode.com/problems/leetflex-banned-accounts)

[中文文档](/solution/1700-1799/1747.Leetflex%20Banned%20Accounts/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>LogInfo</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| account_id  | int      |
| ip_address  | int      |
| login       | datetime |
| logout      | datetime |
+-------------+----------+
Bảng này có thể chứa các hàng trùng lặp.
Bảng chứa thông tin về thời điểm đăng nhập và đăng xuất của các tài khoản Leetflex, cùng với địa chỉ IP mà tài khoản đã dùng để đăng nhập và đăng xuất.
Đảm bảo thời điểm đăng xuất sau thời điểm đăng nhập.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm <code>account_id</code> của các tài khoản cần bị cấm khỏi Leetflex. Một tài khoản cần bị cấm nếu tại một thời điểm nào đó nó đang đăng nhập từ hai địa chỉ IP khác nhau.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
LogInfo table:
+------------+------------+---------------------+---------------------+
| account_id | ip_address | login               | logout              |
+------------+------------+---------------------+---------------------+
| 1          | 1          | 2021-02-01 09:00:00 | 2021-02-01 09:30:00 |
| 1          | 2          | 2021-02-01 08:00:00 | 2021-02-01 11:30:00 |
| 2          | 6          | 2021-02-01 20:30:00 | 2021-02-01 22:00:00 |
| 2          | 7          | 2021-02-02 20:30:00 | 2021-02-02 22:00:00 |
| 3          | 9          | 2021-02-01 16:00:00 | 2021-02-01 16:59:59 |
| 3          | 13         | 2021-02-01 17:00:00 | 2021-02-01 17:59:59 |
| 4          | 10         | 2021-02-01 16:00:00 | 2021-02-01 17:00:00 |
| 4          | 11         | 2021-02-01 17:00:00 | 2021-02-01 17:59:59 |
+------------+------------+---------------------+---------------------+
<strong>Đầu ra:</strong>
+------------+
| account_id |
+------------+
| 1          |
| 4          |
+------------+
<strong>Giải thích:</strong>
Account ID 1 --&gt; Tài khoản hoạt động từ &quot;2021-02-01 09:00:00&quot; đến &quot;2021-02-01 09:30:00&quot; với hai địa chỉ IP khác nhau (1 và 2). Tài khoản này cần bị cấm.
Account ID 2 --&gt; Tài khoản hoạt động từ hai địa chỉ khác nhau (6, 7) nhưng vào <strong>hai thời điểm khác nhau</strong>.
Account ID 3 --&gt; Tài khoản hoạt động từ hai địa chỉ khác nhau (9, 13) trong cùng ngày nhưng <strong>không giao nhau tại bất kỳ thời điểm nào</strong>.
Account ID 4 --&gt; Tài khoản hoạt động từ &quot;2021-02-01 17:00:00&quot; đến &quot;2021-02-01 17:00:00&quot; với hai địa chỉ IP khác nhau (10 và 11). Tài khoản này cần bị cấm.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Self-Join

<!-- thinking:start -->

> **Tư duy**
>
> Một tài khoản bị cấm nếu hai phiên chồng lấn sử dụng các IP khác nhau. Ta cần tìm các account id đó.
>
> Self-join $\textit{LogInfo}$ với cùng tài khoản, IP khác nhau và một $\textit{login}$ nằm trong khoảng $[\textit{login},\textit{logout}]$ của bản ghi kia. Lấy các id không trùng nhau.

<!-- thinking:end -->

Ta có thể dùng self-join để tìm các trường hợp một tài khoản đăng nhập từ những địa chỉ IP khác nhau trong cùng thời gian. Điều kiện join là:

- Số tài khoản giống nhau.
- Địa chỉ IP khác nhau.
- Thời điểm đăng nhập của một bản ghi nằm trong khoảng đăng nhập-đăng xuất của bản ghi kia.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DISTINCT
    a.account_id
FROM
    LogInfo AS a
    JOIN LogInfo AS b
        ON a.account_id = b.account_id
        AND a.ip_address != b.ip_address
        AND a.login BETWEEN b.login AND b.logout;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
