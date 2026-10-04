---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2720. Popularity Percentage 🔒](https://leetcode.com/problems/popularity-percentage)

[中文文档](/solution/2700-2799/2720.Popularity%20Percentage/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Friends</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| user1       | int  |
| user2       | int  |
+-------------+------+
(user1, user2) là khóa chính (tổ hợp các giá trị duy nhất) của bảng này.
Mỗi hàng chứa thông tin về một quan hệ bạn bè, trong đó user1 và user2 là bạn của nhau.
</pre>

<p>Viết một lời giải để tìm phần trăm mức độ phổ biến của mỗi người dùng trên Meta/Facebook. Phần trăm mức độ phổ biến được định nghĩa là tổng số bạn bè mà người dùng đó có chia cho tổng số người dùng trên nền tảng, sau đó chuyển thành phần trăm bằng cách nhân với 100, <strong>làm tròn đến 2 chữ số thập phân</strong>.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo</em> <code>user1</code> <em>theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>&nbsp;
Bảng Friends:
+-------+-------+
| user1 | user2 |
+-------+-------+
| 2 &nbsp; &nbsp; | 1 &nbsp; &nbsp; |
| 1 &nbsp; &nbsp; | 3 &nbsp; &nbsp; |
| 4 &nbsp; &nbsp; | 1 &nbsp; &nbsp; |
| 1 &nbsp; &nbsp; | 5 &nbsp; &nbsp; |
| 1 &nbsp; &nbsp; | 6 &nbsp; &nbsp; |
| 2 &nbsp; &nbsp; | 6 &nbsp; &nbsp; |
| 7 &nbsp; &nbsp; | 2 &nbsp; &nbsp; |
| 8 &nbsp; &nbsp; | 3&nbsp; &nbsp; &nbsp;|
| 3 &nbsp; &nbsp; | 9 &nbsp; &nbsp; |
+-------+-------+
<strong>Đầu ra:</strong>&nbsp;
+-------+-----------------------+
| user1 | percentage_popularity |
+-------+-----------------------+
| 1     | 55.56 &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|
| 2     | 33.33 &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|
| 3     | 33.33   &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| 4     | 11.11 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| 5     | 11.11 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| 6     | 22.22 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| 7     | 11.11 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| 8     | 11.11 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| 9     | 11.11 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
+-------+-----------------------+
<strong>Giải thích:</strong>&nbsp;
Có tổng cộng 9 người dùng trên nền tảng.
- Người dùng "1" có quan hệ bạn bè với 2, 3, 4, 5 và 6. Vì vậy, phần trăm mức độ phổ biến của người dùng 1 được tính như sau: (5/9) * 100 = 55.56.
- Người dùng "2" có quan hệ bạn bè với 1, 6 và 7. Vì vậy, phần trăm mức độ phổ biến của người dùng 2 được tính như sau: (3/9) * 100 = 33.33.
- Người dùng "3" có quan hệ bạn bè với 1, 8 và 9. Vì vậy, phần trăm mức độ phổ biến của người dùng 3 được tính như sau: (3/9) * 100 = 33.33.
- Người dùng "4" có quan hệ bạn bè với 1. Vì vậy, phần trăm mức độ phổ biến của người dùng 4 được tính như sau: (1/9) * 100 = 11.11.
- Người dùng "5" có quan hệ bạn bè với 1. Vì vậy, phần trăm mức độ phổ biến của người dùng 5 được tính như sau: (1/9) * 100 = 11.11.
- Người dùng "6" có quan hệ bạn bè với 1 và 2. Vì vậy, phần trăm mức độ phổ biến của người dùng 6 được tính như sau: (2/9) * 100 = 22.22.
- Người dùng "7" có quan hệ bạn bè với 2. Vì vậy, phần trăm mức độ phổ biến của người dùng 7 được tính như sau: (1/9) * 100 = 11.11.
- Người dùng "8" có quan hệ bạn bè với 3. Vì vậy, phần trăm mức độ phổ biến của người dùng 8 được tính như sau: (1/9) * 100 = 11.11.
- Người dùng "9" có quan hệ bạn bè với 3. Vì vậy, phần trăm mức độ phổ biến của người dùng 9 được tính như sau: (1/9) * 100 = 11.11.
user1 được sắp xếp theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mức độ phổ biến là số bạn bè của một người dùng chia cho số người dùng. Nếu chỉ lưu một chiều, ta sẽ bỏ sót những cạnh mà người dùng là $user2$, đồng thời có thể đếm một cặp hai lần.
>
> Gộp $(user2,user1)$ vào $F$ để biến đồ thị thành vô hướng. Số người dùng là số lượng $user1$ khác nhau trong $F$; một window đếm bậc của từng người dùng, chia cho tổng đó rồi làm tròn đến hai chữ số thập phân.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    F AS (
        SELECT * FROM Friends
        UNION
        SELECT user2, user1 FROM Friends
    ),
    T AS (SELECT COUNT(DISTINCT user1) AS cnt FROM F)
SELECT DISTINCT
    user1,
    ROUND(
        (COUNT(1) OVER (PARTITION BY user1)) * 100 / (SELECT cnt FROM T),
        2
    ) AS percentage_popularity
FROM F
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
