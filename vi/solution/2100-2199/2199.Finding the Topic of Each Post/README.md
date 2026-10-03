---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2199. Finding the Topic of Each Post 🔒](https://leetcode.com/problems/finding-the-topic-of-each-post)

[中文文档](/solution/2100-2199/2199.Finding%20the%20Topic%20of%20Each%20Post/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Keywords</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| topic_id    | int     |
| word        | varchar |
+-------------+---------+
(topic_id, word) là khóa chính (tổ hợp các cột có giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này chứa ID của một chủ đề và một từ được dùng để thể hiện chủ đề đó.
Có thể có nhiều từ cùng thể hiện một chủ đề, và một từ có thể được dùng để thể hiện nhiều chủ đề.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Posts</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| post_id     | int     |
| content     | varchar |
+-------------+---------+
post_id là khóa chính (cột có giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này chứa ID của một bài đăng và nội dung của bài đăng đó.
Nội dung chỉ gồm các chữ cái tiếng Anh và dấu cách.
</pre>

<p>&nbsp;</p>

<p>LeetCode đã thu thập một số bài đăng từ mạng xã hội của mình và muốn tìm chủ đề của từng bài đăng. Mỗi chủ đề có thể được thể hiện bởi một hoặc nhiều từ khóa. Nếu một từ khóa của một chủ đề xuất hiện trong nội dung của bài đăng (<strong>không phân biệt hoa thường</strong>), bài đăng đó có chủ đề này.</p>

<p>Hãy viết lời giải để tìm chủ đề của từng bài đăng theo các quy tắc sau:</p>

<ul>
	<li>Nếu bài đăng không có từ khóa nào thuộc bất kỳ chủ đề nào, chủ đề của bài đăng phải là <code>&quot;Ambiguous!&quot;</code>.</li>
	<li>Nếu bài đăng có ít nhất một từ khóa thuộc bất kỳ chủ đề nào, chủ đề của bài đăng phải là chuỗi các ID của những chủ đề đó, được sắp xếp theo thứ tự tăng dần và phân tách bằng dấu phẩy <code>&#39;,&#39;</code>. Chuỗi không được chứa các ID trùng lặp.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Keywords:
+----------+----------+
| topic_id | word     |
+----------+----------+
| 1        | handball |
| 1        | football |
| 3        | WAR      |
| 2        | Vaccine  |
+----------+----------+
Bảng Posts:
+---------+------------------------------------------------------------------------+
| post_id | content                                                                |
+---------+------------------------------------------------------------------------+
| 1       | We call it soccer They call it football hahaha                         |
| 2       | Americans prefer basketball while Europeans love handball and football |
| 3       | stop the war and play handball                                         |
| 4       | warning I planted some flowers this morning and then got vaccinated    |
+---------+------------------------------------------------------------------------+
<strong>Đầu ra:</strong>
+---------+------------+
| post_id | topic      |
+---------+------------+
| 1       | 1          |
| 2       | 1          |
| 3       | 1,3        |
| 4       | Ambiguous! |
+---------+------------+
<strong>Giải thích:</strong>
1: &quot;We call it soccer They call it football hahaha&quot;
&quot;football&quot; thể hiện chủ đề 1. Không có từ nào khác thể hiện bất kỳ chủ đề nào khác.

2: &quot;Americans prefer basketball while Europeans love handball and football&quot;
&quot;handball&quot; thể hiện chủ đề 1. &quot;football&quot; thể hiện chủ đề 1.
Không có từ nào khác thể hiện bất kỳ chủ đề nào khác.

3: &quot;stop the war and play handball&quot;
&quot;war&quot; thể hiện chủ đề 3. &quot;handball&quot; thể hiện chủ đề 1.
Không có từ nào khác thể hiện bất kỳ chủ đề nào khác.

4: &quot;warning I planted some flowers this morning and then got vaccinated&quot;
Không có từ nào trong câu này thể hiện bất kỳ chủ đề nào. Lưu ý rằng &quot;warning&quot; khác với &quot;war&quot; dù chúng có chung tiền tố.
Bài đăng này không xác định được chủ đề.

Lưu ý rằng một từ có thể thể hiện nhiều chủ đề.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chủ đề của một bài đăng là các từ khóa xuất hiện dưới dạng từ hoàn chỉnh trong nội dung; nếu không thì chủ đề là `Ambiguous!`. Kiểm tra chuỗi con trực tiếp có thể khớp với một phần của từ dài hơn.
>
> Thêm dấu cách vào đầu và cuối cả nội dung lẫn từ khóa, sau đó dùng $\texttt{INSTR}$ để kiểm tra từ hoàn chỉnh. Phép nối trái giữ lại các bài đăng không khớp với từ khóa nào. Nhóm theo $\textit{post\_id}$ và nối các $\textit{topic\_id}$ không trùng lặp, thay danh sách null bằng nhãn mặc định.
>
> $\texttt{GROUP\_CONCAT(DISTINCT\ldots)}$ tạo danh sách các chủ đề.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    post_id,
    IFNULL(GROUP_CONCAT(DISTINCT topic_id), 'Ambiguous!') AS topic
FROM
    Posts
    LEFT JOIN Keywords ON INSTR(CONCAT(' ', content, ' '), CONCAT(' ', word, ' ')) > 0
GROUP BY post_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
