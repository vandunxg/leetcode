---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2990. Loan Types 🔒](https://leetcode.com/problems/loan-types)

[中文文档](/solution/2900-2999/2990.Loan%20Types/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Loans</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| loan_id     | int     |
| user_id     | int     |
| loan_type   | varchar |
+-------------+---------+
loan_id là cột chứa các giá trị duy nhất trong bảng này.
Bảng này chứa loan_id, user_id và loan_type.
</pre>

<p>Hãy viết lời giải để tìm tất cả <strong>khác nhau</strong> <code>user_id</code> có <strong>ít nhất một</strong> khoản vay loại <strong>Refinance</strong> và ít nhất một khoản vay loại <strong>Mortgage</strong>.</p>

<p><em>Trả về bảng kết quả theo thứ tự </em><code>user_id</code><em> <strong>tăng dần</strong></em><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Loans:
+---------+---------+-----------+
| loan_id | user_id | loan_type |
+---------+---------+-----------+
| 683     | 101     | Mortgage  |
| 218     | 101     | AutoLoan  |
| 802     | 101     | Inschool  |
| 593     | 102     | Mortgage  |
| 138     | 102     | Refinance |
| 294     | 102     | Inschool  |
| 308     | 103     | Refinance |
| 389     | 104     | Mortgage  |
+---------+---------+-----------+
<strong>Đầu ra</strong>
+---------+
| user_id |
+---------+
| 102     |
+---------+
<strong>Giải thích</strong>
- user_id 101 có ba loại khoản vay, một trong số đó là Mortgage. Tuy nhiên, user này không có khoản vay nào thuộc loại Refinance, nên user_id 101 không được xét.
- user_id 102 có ba loại khoản vay: một khoản Mortgage và một khoản Refinance. Vì vậy, user_id 102 được đưa vào kết quả.
- user_id 103 có khoản vay loại Refinance nhưng không có khoản vay Mortgage, nên user_id 103 không được xét.
- user_id 104 có khoản vay Mortgage nhưng không có khoản vay Refinance, vì vậy user_id 104 không được xét.
Bảng kết quả được sắp xếp theo user_id tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Một user phải có cả Refinance và Mortgage. Nhóm theo $user_id$ và kiểm tra đồng thời $SUM(loan_type='Refinance')$ cùng với điều kiện tương tự cho Mortgage.
>
> Cách này ngắn hơn việc dùng hai truy vấn con kiểm tra tồn tại hoặc self-join. Sắp xếp theo user id.

<!-- thinking:end -->

Ta có thể nhóm bảng `Loans` theo `user_id` để tìm những user có cả `Refinance` và `Mortgage`. Sau đó, sắp xếp kết quả theo `user_id`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT user_id
FROM Loans
GROUP BY 1
HAVING SUM(loan_type = 'Refinance') > 0 AND SUM(loan_type = 'Mortgage') > 0
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
