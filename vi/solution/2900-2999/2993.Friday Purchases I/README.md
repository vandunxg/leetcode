---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2993. Friday Purchases I 🔒](https://leetcode.com/problems/friday-purchases-i)

[中文文档](/solution/2900-2999/2993.Friday%20Purchases%20I/README.md)

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
(user_id, purchase_date, amount_spend) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng.
purchase_date có giá trị từ ngày 1 tháng 11 năm 2023 đến ngày 30 tháng 11 năm 2023, bao gồm cả hai ngày.
Mỗi hàng chứa mã người dùng, ngày mua và số tiền chi tiêu.
</pre>

<p>Viết lời giải để tính <strong>tổng chi tiêu</strong> của người dùng vào <strong>mỗi thứ Sáu</strong> trong <strong>từng tuần</strong> của <strong>tháng 11 năm 2023</strong>. Chỉ trả về những tuần có <strong>ít nhất một</strong> lượt mua vào <strong>thứ Sáu</strong>.</p>

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
| 4             | 2023-11-24    | 21692        |
+---------------+---------------+--------------+
<strong>Giải thích:</strong>
- Trong tuần đầu tiên của tháng 11 năm 2023, đã có các giao dịch với tổng số tiền là $5,117 vào thứ Sáu, ngày 2023-11-03.
- Trong tuần thứ hai của tháng 11 năm 2023, không có giao dịch nào vào thứ Sáu, ngày 2023-11-10.
- Tương tự, trong tuần thứ ba của tháng 11 năm 2023, không có giao dịch nào vào thứ Sáu, ngày 2023-11-17.
- Trong tuần thứ tư của tháng 11 năm 2023, có hai giao dịch diễn ra vào thứ Sáu, ngày 2023-11-24, lần lượt có số tiền là $12,000 and $9,692, tổng cộng là $21,692.
Bảng kết quả được sắp xếp theo week_of_month theo thứ tự tăng dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Các hàm ngày tháng

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ lấy các ngày thứ Sáu trong tháng 11 năm 2023 và tổng hợp theo tuần trong tháng. $DATE_FORMAT$ cố định tháng, $DAYOFWEEK=6$ chọn thứ Sáu, còn $CEIL(DAYOFMONTH/7)$ là chỉ số tuần.
>
> Nhóm theo ngày, tính tổng và sắp xếp theo tuần. Những ngày thứ Sáu không có trong bảng sẽ không xuất hiện.

<!-- thinking:end -->

Các hàm ngày tháng được sử dụng gồm:

- `DATE_FORMAT(date, format)`: Định dạng một ngày thành chuỗi
- `DAYOFWEEK(date)`: Trả về thứ trong tuần của một ngày, trong đó 1 là Chủ nhật, 2 là thứ Hai, v.v.
- `DAYOFMONTH(date)`: Trả về ngày trong tháng của một ngày

Trước tiên, chúng ta sử dụng hàm `DATE_FORMAT` để định dạng ngày theo dạng `YYYYMM`, sau đó lọc các bản ghi thuộc tháng 11 năm 2023 và rơi vào thứ Sáu. Tiếp theo, chúng ta nhóm các bản ghi theo `purchase_date` và tính tổng số tiền chi tiêu cho mỗi thứ Sáu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    CEIL(DAYOFMONTH(purchase_date) / 7) AS week_of_month,
    purchase_date,
    SUM(amount_spend) AS total_amount
FROM Purchases
WHERE DATE_FORMAT(purchase_date, '%Y%m') = '202311' AND DAYOFWEEK(purchase_date) = 6
GROUP BY 2
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
