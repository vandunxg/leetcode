---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1435. Create a Session Bar Chart 🔒](https://leetcode.com/problems/create-a-session-bar-chart)

[中文文档](/solution/1400-1499/1435.Create%20a%20Session%20Bar%20Chart/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Sessions</code></p>

<pre>
+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| session_id          | int     |
| duration            | int     |
+---------------------+---------+
session_id là cột chứa các giá trị duy nhất của bảng này.
duration là thời gian tính bằng giây mà người dùng đã truy cập ứng dụng.
</pre>

<p>&nbsp;</p>

<p>Bạn muốn biết người dùng truy cập ứng dụng trong bao lâu. Bạn quyết định tạo các khoảng <code>&quot;[0-5&gt;&quot;</code>, <code>&quot;[5-10&gt;&quot;</code>, &quot;[10-15&gt;&quot;, và <code>&quot;15 minutes or more&quot;</code>, rồi đếm số session trong mỗi khoảng.</p>

<p>Hãy viết lời giải để trả về <code>(bin, total)</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Sessions:
+-------------+---------------+
| session_id  | duration      |
+-------------+---------------+
| 1           | 30            |
| 2           | 199           |
| 3           | 299           |
| 4           | 580           |
| 5           | 1000          |
+-------------+---------------+
<strong>Đầu ra:</strong>
+--------------+--------------+
| bin          | total        |
+--------------+--------------+
| [0-5&gt;        | 3            |
| [5-10&gt;       | 1            |
| [10-15&gt;      | 0            |
| 15 or more   | 1            |
+--------------+--------------+
<strong>Giải thích:</strong>
session_id 1, 2 và 3 có duration lớn hơn hoặc bằng 0 phút và nhỏ hơn 5 phút.
session_id 4 có duration lớn hơn hoặc bằng 5 phút và nhỏ hơn 10 phút.
Không có session nào có duration lớn hơn hoặc bằng 10 phút và nhỏ hơn 15 phút.
session_id 5 có duration lớn hơn hoặc bằng 15 phút.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các session phải được phân vào bốn khoảng nửa kín, bao gồm cả những khoảng rỗng. Một `GROUP BY` duy nhất trên `CASE` có thể bỏ qua các khoảng rỗng, vì vậy chúng ta dùng `UNION` cho bốn truy vấn `COUNT`, mỗi truy vấn tương ứng với một khoảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT '[0-5>' AS bin, COUNT(1) AS total FROM Sessions WHERE duration < 300
UNION
SELECT '[5-10>' AS bin, COUNT(1) AS total FROM Sessions WHERE 300 <= duration AND duration < 600
UNION
SELECT '[10-15>' AS bin, COUNT(1) AS total FROM Sessions WHERE 600 <= duration AND duration < 900
UNION
SELECT '15 or more' AS bin, COUNT(1) AS total FROM Sessions WHERE 900 <= duration;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
