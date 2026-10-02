---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1495. Friendly Movies Streamed Last Month 🔒](https://leetcode.com/problems/friendly-movies-streamed-last-month)

[中文文档](/solution/1400-1499/1495.Friendly%20Movies%20Streamed%20Last%20Month/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>TVProgram</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| program_date  | date    |
| content_id    | int     |
| channel       | varchar |
+---------------+---------+
(program_date, content_id) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Bảng này chứa thông tin về các chương trình trên TV.
content_id là id của chương trình trên một kênh nào đó của TV.</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Content</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| content_id       | varchar |
| title            | varchar |
| Kids_content     | enum    |
| content_type     | varchar |
+------------------+---------+
content_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Kids_content là một ENUM (nhóm) gồm các giá trị (&#39;Y&#39;, &#39;N&#39;), trong đó:
&#39;Y&#39; nghĩa là nội dung dành cho trẻ em, còn &#39;N&#39; nghĩa là không dành cho trẻ em.
content_type là loại nội dung, chẳng hạn như phim, series, v.v.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để trả về các tiêu đề không trùng nhau của những bộ phim thân thiện với trẻ em được phát trong <strong>tháng 6 năm 2020</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng TVProgram:
+--------------------+--------------+-------------+
| program_date       | content_id   | channel     |
+--------------------+--------------+-------------+
| 2020-06-10 08:00   | 1            | LC-Channel  |
| 2020-05-11 12:00   | 2            | LC-Channel  |
| 2020-05-12 12:00   | 3            | LC-Channel  |
| 2020-05-13 14:00   | 4            | Disney Ch   |
| 2020-06-18 14:00   | 4            | Disney Ch   |
| 2020-07-15 16:00   | 5            | Disney Ch   |
+--------------------+--------------+-------------+
Bảng Content:
+------------+----------------+---------------+---------------+
| content_id | title          | Kids_content  | content_type  |
+------------+----------------+---------------+---------------+
| 1          | Leetcode Movie | N             | Movies        |
| 2          | Alg. for Kids  | Y             | Series        |
| 3          | Database Sols  | N             | Series        |
| 4          | Aladdin        | Y             | Movies        |
| 5          | Cinderella     | Y             | Movies        |
+------------+----------------+---------------+---------------+
<strong>Đầu ra:</strong>
+--------------+
| title        |
+--------------+
| Aladdin      |
+--------------+
<strong>Giải thích:</strong>
&quot;Leetcode Movie&quot; không phải là nội dung dành cho trẻ em.
&quot;Alg. for Kids&quot; không phải là một bộ phim.
&quot;Database Sols&quot; không phải là một bộ phim.
&quot;Alladin&quot; là một bộ phim dành cho trẻ em và được phát trong tháng 6 năm 2020.
&quot;Cinderella&quot; không được phát trong tháng 6 năm 2020.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Equi-Join + Lọc có điều kiện

<!-- thinking:start -->

> **Tư duy**
>
> Join `TVProgram` với `Content` theo `content_id`, giữ lại các bộ phim dành cho trẻ em trong tháng 6 năm $2020$, sau đó dùng `DISTINCT` cho các tiêu đề.

<!-- thinking:end -->

Trước tiên, ta có thể dùng equi-join để nối hai bảng dựa trên trường `content_id`, sau đó dùng bộ lọc điều kiện để chọn các bộ phim dành cho trẻ em được phát trong tháng 6 năm 2020.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT DISTINCT title
FROM
    TVProgram
    JOIN Content USING (content_id)
WHERE
    DATE_FORMAT(program_date, '%Y%m') = '202006'
    AND kids_content = 'Y'
    AND content_type = 'Movies';
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
