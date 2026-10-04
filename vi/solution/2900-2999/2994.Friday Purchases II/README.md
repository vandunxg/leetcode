---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2994. Friday Purchases II 🔒](https://leetcode.com/problems/friday-purchases-ii)

[中文文档](/solution/2900-2999/2994.Friday%20Purchases%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Purchases</code></p>

<pre>
+---------------+------+
| Column Name   | Type |
+---------------+------+
| user_id       | int  |
| purchase_date | date |
| amount_spend  | int  |
+---------------+------+
(user_id, purchase_date, amount_spend) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
purchase_date nằm trong khoảng từ ngày 1 tháng 11 năm 2023 đến ngày 30 tháng 11 năm 2023, bao gồm cả hai ngày.
Mỗi hàng chứa ID người dùng, ngày mua hàng và số tiền đã chi.
</pre>

<p>Hãy viết lời giải để tính <strong>tổng chi tiêu</strong> của người dùng vào <strong>mỗi thứ Sáu</strong> trong <strong>từng tuần</strong> của <strong>tháng 11 năm 2023</strong>. Nếu một <strong>thứ Sáu trong tuần</strong> không có <strong>giao dịch mua hàng</strong> nào thì được xem là <code>0</code>.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo tuần trong tháng</em><em> theo thứ tự <strong>tăng dần</strong></em><em><strong> </strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Purchases:
+---------+---------------+--------------+
| user_id | purchase_date | amount_spend |
+---------+---------------+--------------+
| 11      | 2023-11-07    | 1126         |
| 15      | 2023-11-30    | 7473         |
| 17      | 2023-11-14    | 2414         |
| 12      | 2023-11-24    | 9692         |
| 8       | 2023-11-03    | 5117         |
| 1       | 2023-11-16    | 5241         |
| 10      | 2023-11-12    | 8266         |
| 13      | 2023-11-24    | 12000        |
+---------+---------------+--------------+
<strong>Đầu ra:</strong>
+---------------+---------------+--------------+
| week_of_month | purchase_date | total_amount |
+---------------+---------------+--------------+
| 1             | 2023-11-03    | 5117         |
| 2             | 2023-11-10    | 0            |
| 3             | 2023-11-17    | 0            |
| 4             | 2023-11-24    | 21692        |
+---------------+---------------+--------------+
<strong>Giải thích:</strong>
- Trong tuần đầu tiên của tháng 11 năm 2023, đã có các giao dịch với tổng số tiền là $5,117 vào thứ Sáu, ngày 2023-11-03.
- Trong tuần thứ hai của tháng 11 năm 2023, không có giao dịch nào vào thứ Sáu, ngày 2023-11-10, nên giá trị của ngày đó trong bảng kết quả là 0.
- Tương tự, trong tuần thứ ba của tháng 11 năm 2023, không có giao dịch nào vào thứ Sáu, ngày 2023-11-17, nên giá trị của ngày đó trong bảng kết quả là 0.
- Trong tuần thứ tư của tháng 11 năm 2023, có hai giao dịch diễn ra vào thứ Sáu, ngày 2023-11-24, với số tiền lần lượt là $12,000 and $9,692, tổng cộng là $21,692.
Bảng kết quả được sắp xếp theo week_of_month theo thứ tự tăng dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy + Left Join + Hàm ngày tháng

<!-- thinking:start -->

> **Tư duy**
>
> Khác với phần I, các ngày thứ Sáu không có giao dịch phải hiển thị $0$. Một CTE đệ quy liệt kê mọi ngày trong tháng 11, left join với $Purchases$, giữ lại các ngày thứ Sáu và dùng $IFNULL(SUM,0)$.
>
> Trục ngày vẫn đầy đủ, nên các ngày thứ Sáu không có giao dịch sẽ không bị loại khỏi phép nhóm.

<!-- thinking:end -->

Ta có thể tạo một bảng `T` chứa tất cả các ngày trong tháng 11 năm 2023 bằng đệ quy, sau đó dùng left join để nối `T` với bảng `Purchases` theo ngày. Cuối cùng, nhóm và tính tổng theo yêu cầu của bài toán.

<!-- tabs:start -->

#### MySQL

```sql
WITH RECURSIVE
    T AS (
        SELECT '2023-11-01' AS purchase_date
        UNION
        SELECT purchase_date + INTERVAL 1 DAY
        FROM T
        WHERE purchase_date < '2023-11-30'
    )
SELECT
    CEIL(DAYOFMONTH(purchase_date) / 7) AS week_of_month,
    purchase_date,
    IFNULL(SUM(amount_spend), 0) AS total_amount
FROM
    T
    LEFT JOIN Purchases USING (purchase_date)
WHERE DAYOFWEEK(purchase_date) = 6
GROUP BY 2
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
