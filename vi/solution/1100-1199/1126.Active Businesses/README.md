---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1126. Active Businesses 🔒](https://leetcode.com/problems/active-businesses)

[中文文档](/solution/1100-1199/1126.Active%20Businesses/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Events</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| business_id   | int     |
| event_type    | varchar |
| occurrences   | int     | 
+---------------+---------+
(business_id, event_type) là khóa chính của bảng này (kết hợp các cột có giá trị duy nhất).
Mỗi hàng ghi lại số lần một loại sự kiện xảy ra tại một doanh nghiệp.
</pre>

<p><strong>Hoạt động trung bình</strong> của một <code>event_type</code> là giá trị trung bình của <code>occurrences</code> trên tất cả doanh nghiệp có sự kiện này.</p>

<p><strong>Doanh nghiệp hoạt động tích cực</strong> là doanh nghiệp có <strong>nhiều hơn một</strong> <code>event_type</code> mà giá trị <code>occurrences</code> của chúng <strong>lớn hơn nghiêm ngặt</strong> mức hoạt động trung bình của sự kiện tương ứng.</p>

<p>Hãy viết lời giải để tìm tất cả <strong>doanh nghiệp hoạt động tích cực</strong>.</p>

<p>Trả về bảng kết quả theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> 
Bảng Events:
+-------------+------------+-------------+
| business_id | event_type | occurrences |
+-------------+------------+-------------+
| 1           | reviews    | 7           |
| 3           | reviews    | 3           |
| 1           | ads        | 11          |
| 2           | ads        | 7           |
| 3           | ads        | 6           |
| 1           | page views | 3           |
| 2           | page views | 12          |
+-------------+------------+-------------+
<strong>Output:</strong> 
+-------------+
| business_id |
+-------------+
| 1           |
+-------------+
<strong>Giải thích:</strong>  
Hoạt động trung bình của mỗi sự kiện được tính như sau:
- &#39;reviews&#39;: (7+3)/2 = 5
- &#39;ads&#39;: (11+7+6)/3 = 8
- &#39;page views&#39;: (3+12)/2 = 7.5
Doanh nghiệp có id=1 có 7 sự kiện &#39;reviews&#39; (lớn hơn 5) và 11 sự kiện &#39;ads&#39; (lớn hơn 8), nên đây là doanh nghiệp hoạt động tích cực.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Doanh nghiệp hoạt động tích cực có `occurences` cao hơn mức trung bình chung ở nhiều hơn một `event_type`. Tính `AVG(occurences)` theo từng loại, join kết quả trở lại bảng, giữ các hàng có giá trị cao hơn mức trung bình, rồi `GROUP BY business_id` với `HAVING COUNT > 1`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT business_id
FROM
    EVENTS AS t1
    JOIN (
        SELECT
            event_type,
            AVG(occurences) AS occurences
        FROM EVENTS
        GROUP BY event_type
    ) AS t2
        ON t1.event_type = t2.event_type
WHERE t1.occurences > t2.occurences
GROUP BY business_id
HAVING COUNT(1) > 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 tính giá trị trung bình trong bảng dẫn xuất rồi join với bảng gốc. `AVG(...) OVER (PARTITION BY event_type)` đánh dấu trực tiếp từng hàng; sau đó lọc và nhóm mà không cần join bổ sung.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            business_id,
            occurences > AVG(occurences) OVER (PARTITION BY event_type) AS mark
        FROM Events
    )
SELECT business_id
FROM T
WHERE mark = 1
GROUP BY 1
HAVING COUNT(1) > 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
