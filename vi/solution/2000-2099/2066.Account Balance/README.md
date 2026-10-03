---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2066. Account Balance 🔒](https://leetcode.com/problems/account-balance)

[中文文档](/solution/2000-2099/2066.Account%20Balance/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Transactions</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| day         | date |
| type        | ENUM |
| amount      | int  |
+-------------+------+
(account_id, day) là khóa chính (tổ hợp các cột có các giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về một giao dịch, bao gồm loại giao dịch, ngày giao dịch và số tiền.
type là kiểu ENUM (danh mục) gồm các giá trị (&#39;Deposit&#39;,&#39;Withdraw&#39;)
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo số dư của mỗi người dùng sau mỗi giao dịch. Bạn có thể giả sử số dư của mỗi tài khoản trước mọi giao dịch là <code>0</code> và số dư sẽ không bao giờ nhỏ hơn <code>0</code> tại bất kỳ thời điểm nào.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự tăng dần</strong> của <code>account_id</code>, sau đó theo <code>day</code> nếu bị trùng.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Transactions:
+------------+------------+----------+--------+
| account_id | day        | type     | amount |
+------------+------------+----------+--------+
| 1          | 2021-11-07 | Deposit  | 2000   |
| 1          | 2021-11-09 | Withdraw | 1000   |
| 1          | 2021-11-11 | Deposit  | 3000   |
| 2          | 2021-12-07 | Deposit  | 7000   |
| 2          | 2021-12-12 | Withdraw | 7000   |
+------------+------------+----------+--------+
<strong>Đầu ra:</strong>
+------------+------------+---------+
| account_id | day        | balance |
+------------+------------+---------+
| 1          | 2021-11-07 | 2000    |
| 1          | 2021-11-09 | 1000    |
| 1          | 2021-11-11 | 4000    |
| 2          | 2021-12-07 | 7000    |
| 2          | 2021-12-12 | 0       |
+------------+------------+---------+
<strong>Giải thích:</strong>
Tài khoản 1:
- Số dư ban đầu là 0.
- 2021-11-07 --&gt; nạp 2000. Số dư là 0 + 2000 = 2000.
- 2021-11-09 --&gt; rút 1000. Số dư là 2000 - 1000 = 1000.
- 2021-11-11 --&gt; nạp 3000. Số dư là 1000 + 3000 = 4000.
Tài khoản 2:
- Số dư ban đầu là 0.
- 2021-12-07 --&gt; nạp 7000. Số dư là 0 + 7000 = 7000.
- 2021-12-12 --&gt; rút 7000. Số dư là 7000 - 7000 = 0.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tính số dư cuối ngày cho từng tài khoản theo thứ tự ngày. Tiền nạp được cộng và tiền rút bị trừ, tạo thành tổng tiền tố trong từng tài khoản.
>
> Một window được phân vùng theo `account_id` và sắp xếp theo `day` sẽ tính tổng các khoản tiền đã đổi dấu.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    account_id,
    day,
    SUM(IF(type = 'Deposit', amount, -amount)) OVER (
        PARTITION BY account_id
        ORDER BY day
    ) AS balance
FROM Transactions
ORDER BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
