---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2205. The Number of Users That Are Eligible for Discount 🔒](https://leetcode.com/problems/the-number-of-users-that-are-eligible-for-discount)

[中文文档](/solution/2200-2299/2205.The%20Number%20of%20Users%20That%20Are%20Eligible%20for%20Discount/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Purchases</code></p>

<pre>
+-------------+----------+
| Tên cột     | Kiểu     |
+-------------+----------+
| user_id     | int      |
| time_stamp  | datetime |
| amount      | int      |
+-------------+----------+
(user_id, time_stamp) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về thời điểm mua hàng và số tiền đã trả của người dùng có ID user_id.
</pre>

<p>&nbsp;</p>

<p>Một người dùng đủ điều kiện nhận giảm giá nếu họ có một giao dịch mua trong khoảng thời gian bao gồm cả hai đầu mút <code>[startDate, endDate]</code> với số tiền ít nhất là <code>minAmount</code>. Khi chuyển đổi ngày thành thời điểm, cả hai ngày đều được xem là <strong>thời điểm bắt đầu</strong> của ngày đó (nghĩa là <code>endDate = 2022-03-05</code> được xem là thời điểm <code>2022-03-05 00:00:00</code>).</p>

<p>Hãy viết lời giải để báo cáo số người dùng đủ điều kiện nhận giảm giá.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Purchases:
+---------+---------------------+--------+
| user_id | time_stamp          | amount |
+---------+---------------------+--------+
| 1       | 2022-04-20 09:03:00 | 4416   |
| 2       | 2022-03-19 19:24:02 | 678    |
| 3       | 2022-03-18 12:03:09 | 4523   |
| 3       | 2022-03-30 09:43:42 | 626    |
+---------+---------------------+--------+
startDate = 2022-03-08, endDate = 2022-03-20, minAmount = 1000
<strong>Đầu ra:</strong>
+----------+
| user_cnt |
+----------+
| 1        |
+----------+
<strong>Giải thích:</strong>
Trong ba người dùng, chỉ người dùng 3 đủ điều kiện nhận giảm giá.
  - Người dùng 1 có một giao dịch mua với số tiền ít nhất là minAmount, nhưng giao dịch đó không nằm trong khoảng thời gian đã cho.
  - Người dùng 2 có một giao dịch mua nằm trong khoảng thời gian đã cho, nhưng số tiền nhỏ hơn minAmount.
  - Người dùng 3 là người duy nhất có một giao dịch mua thỏa mãn cả hai điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Lưu ý quan trọng:</strong> Bài toán này về cơ bản giống với bài <a href="https://leetcode.com/problems/the-users-that-are-eligible-for-discount/description/">The Users That Are Eligible for Discount</a>.</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần đếm những người dùng có ít nhất một giao dịch mua trong $[\textit{startDate}, \textit{endDate}]$ với số tiền ít nhất là $\textit{minAmount}$. Cộng các giao dịch mua của một người dùng sẽ coi nhiều đơn hàng nhỏ là một đơn hàng lớn, trái với yêu cầu của đề bài.
>
> Lọc ở cấp độ hàng: timestamp nằm trong khoảng thời gian và $\textit{amount} \ge \textit{minAmount}$. Loại bỏ trùng lặp bằng $\textit{COUNT}(\textit{DISTINCT user\_id})$ rồi trả về giá trị vô hướng đó từ hàm.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
CREATE FUNCTION getUserIDs(startDate DATE, endDate DATE, minAmount INT) RETURNS INT
BEGIN
  RETURN (
      SELECT COUNT(DISTINCT user_id) AS user_cnt
      FROM Purchases
      WHERE time_stamp BETWEEN startDate AND endDate AND amount >= minAmount
  );
END
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
