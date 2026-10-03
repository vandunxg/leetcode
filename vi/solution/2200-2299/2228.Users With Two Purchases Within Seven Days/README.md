---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2228. Users With Two Purchases Within Seven Days 🔒](https://leetcode.com/problems/users-with-two-purchases-within-seven-days)

[中文文档](/solution/2200-2299/2228.Users%20With%20Two%20Purchases%20Within%20Seven%20Days/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Purchases</code></p>

<pre>
+---------------+------+
| Column Name   | Type |
+---------------+------+
| purchase_id   | int  |
| user_id       | int  |
| purchase_date | date |
+---------------+------+
purchase_id chứa các giá trị không trùng nhau.
Bảng này lưu nhật ký ngày người dùng mua hàng từ một nhà bán lẻ nhất định.
</pre>

<p>&nbsp;</p>

<p>Viết một lời giải để tìm các ID của những người dùng đã thực hiện hai lần mua hàng bất kỳ cách nhau <strong>không quá</strong> <code>7</code> ngày.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>user_id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Purchases:
+-------------+---------+---------------+
| purchase_id | user_id | purchase_date |
+-------------+---------+---------------+
| 4           | 2       | 2022-03-13    |
| 1           | 5       | 2022-02-11    |
| 3           | 7       | 2022-06-19    |
| 6           | 2       | 2022-03-20    |
| 5           | 7       | 2022-06-19    |
| 2           | 2       | 2022-06-08    |
+-------------+---------+---------------+
<strong>Đầu ra:</strong>
+---------+
| user_id |
+---------+
| 2       |
| 7       |
+---------+
<strong>Giải thích:</strong>
Người dùng 2 đã mua hàng vào ngày 2022-03-13 và 2022-03-20. Vì lần mua thứ hai cách lần mua đầu tiên không quá 7 ngày, ta thêm ID của họ.
Người dùng 5 chỉ mua hàng 1 lần.
Người dùng 7 đã mua hàng hai lần trong cùng một ngày, nên ta thêm ID của họ.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm những người dùng có hai lần mua hàng cách nhau không quá $7$ ngày. Tự join mọi cặp bản ghi của cùng một người dùng sẽ đếm thừa khi người đó mua hàng thường xuyên. Sau khi sắp xếp, các lần mua hàng liền kề đã bao quát mọi khoảng thời gian có độ dài $7$.
>
> Dùng $\textit{LAG}(\textit{purchase\_date})$ với nhóm theo $\textit{user\_id}$ và sắp xếp theo ngày sẽ cho khoảng cách tới lần mua trước đó. Giữ lại các dòng có $d \le 7$, rồi lấy các ID người dùng không trùng nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            user_id,
            DATEDIFF(
                purchase_date,
                LAG(purchase_date, 1) OVER (
                    PARTITION BY user_id
                    ORDER BY purchase_date
                )
            ) AS d
        FROM Purchases
    )
SELECT DISTINCT user_id
FROM t
WHERE d <= 7
ORDER BY user_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
