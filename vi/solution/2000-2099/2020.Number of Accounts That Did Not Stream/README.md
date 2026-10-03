---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2020. Number of Accounts That Did Not Stream 🔒](https://leetcode.com/problems/number-of-accounts-that-did-not-stream)

[中文文档](/solution/2000-2099/2020.Number%20of%20Accounts%20That%20Did%20Not%20Stream/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Subscriptions</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| start_date  | date |
| end_date    | date |
+-------------+------+
account_id là cột khóa chính của bảng này.
Mỗi hàng của bảng này cho biết ngày bắt đầu và ngày kết thúc gói đăng ký của một tài khoản.
Lưu ý rằng luôn có start_date &lt; end_date.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Streams</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| session_id  | int  |
| account_id  | int  |
| stream_date | date |
+-------------+------+
session_id là cột khóa chính của bảng này.
account_id là khóa ngoại tham chiếu đến bảng Subscriptions.
Mỗi hàng của bảng này chứa thông tin về tài khoản và ngày diễn ra phiên stream.
</pre>

<p>&nbsp;</p>

<p>Viết một truy vấn SQL để báo cáo số tài khoản đã mua gói đăng ký trong <code>2021</code> nhưng không có bất kỳ phiên stream nào.</p>

<p>Định dạng kết quả truy vấn được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Subscriptions table:
+------------+------------+------------+
| account_id | start_date | end_date   |
+------------+------------+------------+
| 9          | 2020-02-18 | 2021-10-30 |
| 3          | 2021-09-21 | 2021-11-13 |
| 11         | 2020-02-28 | 2020-08-18 |
| 13         | 2021-04-20 | 2021-09-22 |
| 4          | 2020-10-26 | 2021-05-08 |
| 5          | 2020-09-11 | 2021-01-17 |
+------------+------------+------------+
Streams table:
+------------+------------+-------------+
| session_id | account_id | stream_date |
+------------+------------+-------------+
| 14         | 9          | 2020-05-16  |
| 16         | 3          | 2021-10-27  |
| 18         | 11         | 2020-04-29  |
| 17         | 13         | 2021-08-08  |
| 19         | 4          | 2020-12-31  |
| 13         | 5          | 2021-01-05  |
+------------+------------+-------------+
<strong>Đầu ra:</strong>
+----------------+
| accounts_count |
+----------------+
| 2              |
+----------------+
<strong>Giải thích:</strong> Người dùng 4 và 9 không stream trong năm 2021.
Người dùng 11 không đăng ký trong năm 2021.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các tài khoản đã đăng ký trong năm 2021 nhưng không có phiên stream hợp lệ trong năm 2021. Phép left join theo account id giữ lại các gói đăng ký không có phiên stream.
>
> Một gói đăng ký bao phủ năm 2021 khi $start \le 2021 \le end$. Một phiên stream không hợp lệ nếu năm của nó không phải 2021 hoặc nó diễn ra sau `end_date`.
>
> `COUNT` sau đó đếm các hàng thỏa mãn điều kiện `WHERE`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT COUNT(sub.account_id) AS accounts_count
FROM
    Subscriptions AS sub
    LEFT JOIN Streams USING (account_id)
WHERE
    YEAR(start_date) <= 2021
    AND YEAR(end_date) >= 2021
    AND (YEAR(stream_date) != 2021 OR stream_date > end_date);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
