---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1831. Maximum Transaction Each Day 🔒](https://leetcode.com/problems/maximum-transaction-each-day)

[中文文档](/solution/1800-1899/1831.Maximum%20Transaction%20Each%20Day/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Transactions</code></p>

<pre>
+----------------+----------+
| Tên cột        | Kiểu     |
+----------------+----------+
| transaction_id | int      |
| day            | datetime |
| amount         | int      |
+----------------+----------+
transaction_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi dòng chứa thông tin về một giao dịch.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo ID của các giao dịch có <code>amount</code> <strong>lớn nhất</strong> trong ngày tương ứng. Nếu một ngày có nhiều giao dịch như vậy, hãy trả về tất cả chúng.</p>

<p>Trả về bảng kết quả, <strong>sắp xếp theo</strong> <code>transaction_id</code> <strong>theo thứ tự tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Transactions:
+----------------+--------------------+--------+
| transaction_id | day                | amount |
+----------------+--------------------+--------+
| 8              | 2021-4-3 15:57:28  | 57     |
| 9              | 2021-4-28 08:47:25 | 21     |
| 1              | 2021-4-29 13:28:30 | 58     |
| 5              | 2021-4-28 16:39:59 | 40     |
| 6              | 2021-4-29 23:39:28 | 58     |
+----------------+--------------------+--------+
<strong>Đầu ra:</strong>
+----------------+
| transaction_id |
+----------------+
| 1              |
| 5              |
| 6              |
| 8              |
+----------------+
<strong>Giải thích:</strong>
&quot;2021-4-3&quot;  --&gt; Có một giao dịch với ID 8, nên ta thêm 8 vào bảng kết quả.
&quot;2021-4-28&quot; --&gt; Có hai giao dịch với ID 5 và 9. Giao dịch có ID 5 có amount bằng 40, còn giao dịch có ID 9 có amount bằng 21. Ta chỉ đưa giao dịch có ID 5 vào vì đây là giao dịch có amount lớn nhất trong ngày.
&quot;2021-4-29&quot; --&gt; Có hai giao dịch với ID 1 và 6. Cả hai giao dịch có cùng amount là 58, nên ta đưa cả hai vào bảng kết quả.
Sau khi thu thập các ID này, ta sắp xếp bảng kết quả theo transaction_id.
</pre>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này mà không sử dụng hàm <code>MAX()</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần mọi giao dịch đồng hạng với amount lớn nhất trong ngày của nó. Self-join cũng có thể giải quyết, nhưng hàm cửa sổ biểu diễn trực tiếp thứ hạng.
>
> Phân nhóm theo $DAY(day)$, xếp hạng $amount$ theo thứ tự giảm dần, giữ các dòng có hạng $1$, rồi sắp xếp theo $transaction\_id$.

<!-- thinking:end -->

Ta có thể dùng hàm cửa sổ `RANK()`, hàm này gán hạng cho mỗi giao dịch dựa trên amount theo thứ tự giảm dần, sau đó chọn các giao dịch có hạng bằng $1$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            transaction_id,
            RANK() OVER (
                PARTITION BY DAY(day)
                ORDER BY amount DESC
            ) AS rk
        FROM Transactions
    )
SELECT transaction_id
FROM T
WHERE rk = 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
