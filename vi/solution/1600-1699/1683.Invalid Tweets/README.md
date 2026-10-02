---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets)

[中文文档](/solution/1600-1699/1683.Invalid%20Tweets/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Tweets</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| tweet_id       | int     |
| content        | varchar |
+----------------+---------+
tweet_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
content chỉ gồm các ký tự chữ và số, &#39;!&#39; hoặc &#39; &#39;, không có ký tự đặc biệt nào khác.
Bảng này chứa tất cả tweet trong một ứng dụng mạng xã hội.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm ID của các tweet không hợp lệ. Tweet không hợp lệ nếu số ký tự trong nội dung tweet <strong>lớn hơn nghiêm ngặt</strong> <code>15</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong>
Tweets table:
+----------+-----------------------------------+
| tweet_id | content                           |
+----------+-----------------------------------+
| 1        | Let us Code                       |
| 2        | More than fifteen chars are here! |
+----------+-----------------------------------+
<strong>Output:</strong>
+----------+
| tweet_id |
+----------+
| 2        |
+----------+
<strong>Giải thích:</strong>
Tweet 1 có độ dài = 11. Đây là tweet hợp lệ.
Tweet 2 có độ dài = 33. Đây là tweet không hợp lệ.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dùng hàm `CHAR_LENGTH`

<!-- thinking:start -->

> **Tư duy**
>
> Tweet không hợp lệ khi nội dung có hơn $15$ ký tự. Ta cần đếm ký tự chứ không phải độ dài byte, nên dùng $\texttt{CHAR\_LENGTH}$ thay vì $\texttt{LENGTH}$.
>
> Chọn $\texttt{tweet\_id}$ sao cho $\texttt{CHAR\_LENGTH}(\texttt{content})>15$.

<!-- thinking:end -->

Hàm `CHAR_LENGTH()` trả về độ dài chuỗi, trong đó ký tự Trung Quốc, chữ số và chữ cái đều được tính là $1$ ký tự.

Hàm `LENGTH()` trả về độ dài chuỗi theo byte: với mã hóa utf8, ký tự Trung Quốc chiếm $3$ byte còn chữ số và chữ cái chiếm $1$ byte; với mã hóa gbk, ký tự Trung Quốc chiếm $2$ byte còn chữ số và chữ cái chiếm $1$ byte.

Trong bài này, ta có thể dùng trực tiếp hàm `CHAR_LENGTH` để lấy độ dài chuỗi và lọc các tweet có độ dài lớn hơn $15$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    tweet_id
FROM Tweets
WHERE CHAR_LENGTH(content) > 15;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
