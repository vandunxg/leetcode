---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1113. Reported Posts 🔒](https://leetcode.com/problems/reported-posts)

[中文文档](/solution/1100-1199/1113.Reported%20Posts/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Actions</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| post_id       | int     |
| action_date   | date    | 
| action        | enum    |
| extra         | varchar |
+---------------+---------+
Bảng này có thể chứa các hàng trùng lặp.
Cột action có kiểu ENUM (danh mục) với các giá trị (&#39;view&#39;, &#39;like&#39;, &#39;reaction&#39;, &#39;comment&#39;, &#39;report&#39;, &#39;share&#39;).
Cột extra chứa thông tin tùy chọn về hành động, chẳng hạn lý do báo cáo hoặc loại reaction.</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm số bài đăng bị báo cáo vào ngày hôm qua theo từng lý do. Giả sử hôm nay là <code>2019-07-05</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Actions table:
+---------+---------+-------------+--------+--------+
| user_id | post_id | action_date | action | extra  |
+---------+---------+-------------+--------+--------+
| 1       | 1       | 2019-07-01  | view   | null   |
| 1       | 1       | 2019-07-01  | like   | null   |
| 1       | 1       | 2019-07-01  | share  | null   |
| 2       | 4       | 2019-07-04  | view   | null   |
| 2       | 4       | 2019-07-04  | report | spam   |
| 3       | 4       | 2019-07-04  | view   | null   |
| 3       | 4       | 2019-07-04  | report | spam   |
| 4       | 3       | 2019-07-02  | view   | null   |
| 4       | 3       | 2019-07-02  | report | spam   |
| 5       | 2       | 2019-07-04  | view   | null   |
| 5       | 2       | 2019-07-04  | report | racism |
| 5       | 5       | 2019-07-04  | view   | null   |
| 5       | 5       | 2019-07-04  | report | racism |
+---------+---------+-------------+--------+--------+
<strong>Output:</strong> 
+---------------+--------------+
| report_reason | report_count |
+---------------+--------------+
| spam          | 1            |
| racism        | 2            |
+---------------+--------------+
<strong>Giải thích:</strong> Lưu ý, ta chỉ quan tâm đến các lý do báo cáo có số lượt báo cáo lớn hơn 0.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số bài đăng khác nhau theo `extra` (lý do báo cáo) vào ngày đã cho với `action = 'report'`. Trước tiên lọc theo ngày và hành động, sau đó dùng `GROUP BY extra` và `COUNT(DISTINCT post_id)` để mỗi bài đăng dù bị báo cáo nhiều lần cũng chỉ được tính một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT extra AS report_reason, COUNT(DISTINCT post_id) AS report_count
FROM Actions
WHERE action_date = '2019-07-04' AND action = 'report'
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
