---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2051. The Category of Each Member in the Store 🔒](https://leetcode.com/problems/the-category-of-each-member-in-the-store)

[中文文档](/solution/2000-2099/2051.The%20Category%20of%20Each%20Member%20in%20the%20Store/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Members</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| member_id   | int     |
| name        | varchar |
+-------------+---------+
member_id là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng của bảng này cho biết tên và ID của một thành viên.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Visits</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| visit_id    | int  |
| member_id   | int  |
| visit_date  | date |
+-------------+------+
visit_id là cột chứa các giá trị duy nhất trong bảng này.
member_id là khóa ngoại (cột tham chiếu) đến member_id trong bảng Members.
Mỗi hàng của bảng này chứa thông tin về ngày ghé thăm cửa hàng và thành viên đã ghé thăm cửa hàng.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Purchases</code></p>

<pre>
+----------------+------+
| Column Name    | Type |
+----------------+------+
| visit_id       | int  |
| charged_amount | int  |
+----------------+------+
visit_id là cột chứa các giá trị duy nhất trong bảng này.
visit_id là khóa ngoại (cột tham chiếu) đến visit_id trong bảng Visits.
Mỗi hàng của bảng này chứa thông tin về số tiền được tính trong một lần ghé thăm cửa hàng.
</pre>

<p>&nbsp;</p>

<p>Một cửa hàng muốn phân loại các thành viên. Có ba hạng:</p>

<ul>
	<li><strong>&quot;Diamond&quot;</strong>: nếu tỷ lệ chuyển đổi <strong>lớn hơn hoặc bằng</strong> <code>80</code>.</li>
	<li><strong>&quot;Gold&quot;</strong>: nếu tỷ lệ chuyển đổi <strong>lớn hơn hoặc bằng</strong> <code>50</code> và nhỏ hơn <code>80</code>.</li>
	<li><strong>&quot;Silver&quot;</strong>: nếu tỷ lệ chuyển đổi <strong>nhỏ hơn</strong> <code>50</code>.</li>
	<li><strong>&quot;Bronze&quot;</strong>: nếu thành viên chưa từng ghé thăm cửa hàng.</li>
</ul>

<p><strong>Tỷ lệ chuyển đổi</strong> của một thành viên là <code>(100 * total number of purchases for the member) / total number of visits for the member</code>.</p>

<p>Hãy viết lời giải để báo cáo id, tên và hạng của mỗi thành viên.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Members:
+-----------+---------+
| member_id | name    |
+-----------+---------+
| 9         | Alice   |
| 11        | Bob     |
| 3         | Winston |
| 8         | Hercy   |
| 1         | Narihan |
+-----------+---------+
Bảng Visits:
+----------+-----------+------------+
| visit_id | member_id | visit_date |
+----------+-----------+------------+
| 22       | 11        | 2021-10-28 |
| 16       | 11        | 2021-01-12 |
| 18       | 9         | 2021-12-10 |
| 19       | 3         | 2021-10-19 |
| 12       | 11        | 2021-03-01 |
| 17       | 8         | 2021-05-07 |
| 21       | 9         | 2021-05-12 |
+----------+-----------+------------+
Bảng Purchases:
+----------+----------------+
| visit_id | charged_amount |
+----------+----------------+
| 12       | 2000           |
| 18       | 9000           |
| 17       | 7000           |
+----------+----------------+
<strong>Đầu ra:</strong>
+-----------+---------+----------+
| member_id | name    | category |
+-----------+---------+----------+
| 1         | Narihan | Bronze   |
| 3         | Winston | Silver   |
| 8         | Hercy   | Diamond  |
| 9         | Alice   | Gold     |
| 11        | Bob     | Silver   |
+-----------+---------+----------+
<strong>Giải thích:</strong>
- Thành viên Narihan có id = 1 chưa từng ghé thăm cửa hàng. Thành viên này được xếp hạng Bronze.
- Thành viên Winston có id = 3 đã ghé thăm cửa hàng một lần nhưng không mua gì. Tỷ lệ chuyển đổi = (100 * 0) / 1 = 0. Thành viên này được xếp hạng Silver.
- Thành viên Hercy có id = 8 đã ghé thăm cửa hàng một lần và mua hàng một lần. Tỷ lệ chuyển đổi = (100 * 1) / 1 = 1. Thành viên này được xếp hạng Diamond.
- Thành viên Alice có id = 9 đã ghé thăm cửa hàng hai lần và mua hàng một lần. Tỷ lệ chuyển đổi = (100 * 1) / 2 = 50. Thành viên này được xếp hạng Gold.
- Thành viên Bob có id = 11 đã ghé thăm cửa hàng ba lần và mua hàng một lần. Tỷ lệ chuyển đổi = (100 * 1) / 3 = 33.33. Thành viên này được xếp hạng Silver.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hạng được xác định bằng số lượt ghé thăm có mua hàng trên tổng số lượt ghé thăm; nếu chưa từng ghé thăm thì thành viên thuộc hạng Bronze. Có ba bảng, và những thành viên không có lượt ghé thăm vẫn phải được giữ lại.
>
> Nối trái `Visits` với `Purchases`, nhóm theo thành viên: không có lượt ghé thăm $\to$ Bronze, nếu có thì phân loại Diamond / Gold / Silver theo các ngưỡng của tỷ lệ chuyển đổi. `COUNT(charged_amount)` chỉ đếm các giao dịch mua ghép được.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    m.member_id,
    name,
    CASE
        WHEN COUNT(v.visit_id) = 0 THEN 'Bronze'
        WHEN 100 * COUNT(charged_amount) / COUNT(v.visit_id) >= 80 THEN 'Diamond'
        WHEN 100 * COUNT(charged_amount) / COUNT(v.visit_id) >= 50 THEN 'Gold'
        ELSE 'Silver'
    END AS category
FROM
    Members AS m
    LEFT JOIN Visits AS v ON m.member_id = v.member_id
    LEFT JOIN Purchases AS p ON v.visit_id = p.visit_id
GROUP BY member_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
