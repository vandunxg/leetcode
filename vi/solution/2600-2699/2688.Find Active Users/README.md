---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2688. Find Active Users 🔒](https://leetcode.com/problems/find-active-users)

[中文文档](/solution/2600-2699/2688.Find%20Active%20Users/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace">&nbsp;<code>Users</code></font></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| user_id     | int      |
| item        | varchar  |
| created_at  | datetime |
| amount      | int      |
+-------------+----------+
Bảng này có thể chứa các bản ghi trùng lặp.
Mỗi hàng gồm có ID người dùng, sản phẩm đã mua, ngày mua và số tiền mua.
</pre>

<p>Hãy viết lời giải để xác định những người dùng đang hoạt động. Một người dùng đang hoạt động là người đã thực hiện giao dịch mua thứ hai <strong>trong vòng 7&nbsp;ngày</strong> kể từ bất kỳ giao dịch mua nào khác của họ.</p>

<p>Ví dụ, nếu ngày kết thúc là ngày 31 tháng 5 năm 2023, thì mọi ngày từ ngày 31 tháng 5 năm 2023 đến ngày 7 tháng 6 năm 2023 (bao gồm cả hai ngày) được xem là &quot;trong vòng 7 ngày&quot; kể từ ngày 31 tháng 5 năm 2023.</p>

<p>Trả về danh sách <code>user_id</code>, biểu thị danh sách những người dùng đang hoạt động, theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>Bảng Users:
+---------+-------------------+------------+--------+
| user_id | item              | created_at | amount |
+---------+-------------------+------------+--------+
| 5       | Smart Crock Pot   | 2021-09-18 | 698882 |
| 6       | Smart Lock        | 2021-09-14 | 11487  |
| 6       | Smart Thermostat  | 2021-09-10 | 674762 |
| 8       | Smart Light Strip | 2021-09-29 | 630773 |
| 4       | Smart Cat Feeder  | 2021-09-02 | 693545 |
| 4       | Smart Bed         | 2021-09-13 | 170249 |
+---------+-------------------+------------+--------+
<strong>Đầu ra:</strong>
+---------+
| user_id |
+---------+
| 6       |
+---------+
<strong>Giải thích:</strong>
- Người dùng có user_id 5 chỉ có một giao dịch, nên không phải là người dùng đang hoạt động.
- Người dùng có user_id 6 có hai giao dịch: giao dịch đầu tiên vào ngày 2021-09-10 và giao dịch thứ hai vào ngày 2021-09-14. Khoảng cách giữa ngày của hai giao dịch &lt;= 7 ngày, nên đây là người dùng đang hoạt động.
- Người dùng có user_id 8 chỉ có một giao dịch, nên không phải là người dùng đang hoạt động.
- Người dùng có user_id 4 có hai giao dịch: giao dịch đầu tiên vào ngày 2021-09-02 và giao dịch thứ hai vào ngày 2021-09-13. Khoảng cách giữa ngày của hai giao dịch &gt; 7 ngày, nên đây không phải là người dùng đang hoạt động.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một người dùng đang hoạt động có hai giao dịch mua cách nhau không quá $7$ ngày. Thực hiện self-join toàn bộ các ngày sẽ nặng hơn mức cần thiết. Dùng `LAG` trên từng người dùng, sắp xếp theo thời gian, để lấy giao dịch mua trước đó; `DATEDIFF` $\le 7$ xác định người dùng, sau đó dùng `DISTINCT` cho các mã người dùng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement
SELECT DISTINCT
    user_id
FROM Users
WHERE
    user_id IN (
        SELECT
            user_id
        FROM
            (
                SELECT
                    user_id,
                    created_at,
                    LAG(created_at, 1) OVER (
                        PARTITION BY user_id
                        ORDER BY created_at
                    ) AS prev_created_at
                FROM Users
            ) AS t
        WHERE DATEDIFF(created_at, prev_created_at) <= 7
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
