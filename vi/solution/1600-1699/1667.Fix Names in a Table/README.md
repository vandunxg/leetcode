---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1667. Fix Names in a Table](https://leetcode.com/problems/fix-names-in-a-table)

[中文文档](/solution/1600-1699/1667.Fix%20Names%20in%20a%20Table/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Users</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| name           | varchar |
+----------------+---------+
user_id is the primary key (column with unique values) for this table.
This table contains the ID and the name of the user. The name consists of only lowercase and uppercase characters.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải chuẩn hóa tên sao cho chỉ ký tự đầu tiên viết hoa và các ký tự còn lại viết thường.</p>

<p>Trả về bảng kết quả được sắp xếp theo <code>user_id</code>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong>
Users table:
+---------+-------+
| user_id | name  |
+---------+-------+
| 1       | aLice |
| 2       | bOB   |
+---------+-------+
<strong>Output:</strong>
+---------+-------+
| user_id | name  |
+---------+-------+
| 1       | Alice |
| 2       | Bob   |
+---------+-------+
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tên cần có ký tự đầu viết hoa và phần còn lại viết thường. Nối $\texttt{UPPER}(\texttt{LEFT}(name,1))$ với $\texttt{LOWER}(\texttt{SUBSTRING}(name,2))$, sau đó sắp xếp theo $\texttt{user\_id}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    user_id,
    CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM
    users
ORDER BY
    user_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> $\texttt{SUBSTRING}(name,2)$ trong Lời giải 1 lấy đến cuối chuỗi. Một số engine viết cùng phần cắt đó là $\texttt{SUBSTRING}(name,2,\texttt{DATALENGTH}(name))$.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
SELECT
    user_id,
    CONCAT(
        UPPER(LEFT(name, 1)),
        LOWER(SUBSTRING(name, 2, DATALENGTH(name)))
    ) AS name
FROM
    users
ORDER BY
    user_id;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
