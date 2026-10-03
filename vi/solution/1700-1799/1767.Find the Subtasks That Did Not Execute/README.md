---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [1767. Find the Subtasks That Did Not Execute 🔒](https://leetcode.com/problems/find-the-subtasks-that-did-not-execute)

[中文文档](/solution/1700-1799/1767.Find%20the%20Subtasks%20That%20Did%20Not%20Execute/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Tasks</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| task_id        | int     |
| subtasks_count | int     |
+----------------+---------+
task_id là cột có các giá trị duy nhất trong bảng này.
Mỗi dòng cho biết task_id được chia thành subtasks_count subtask được đánh nhãn từ 1 đến subtasks_count.
Đảm bảo rằng 2 &lt;= subtasks_count &lt;= 20.
</pre>

<p>&nbsp;</p>

<p>Table: <code>Executed</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| task_id       | int     |
| subtask_id    | int     |
+---------------+---------+
(task_id, subtask_id) là tổ hợp các cột có giá trị duy nhất trong bảng này.
Mỗi dòng cho biết subtask có ID subtask_id của task task_id đã được thực thi thành công.
<strong>Đảm bảo</strong> rằng subtask_id &lt;= subtasks_count với mỗi task_id.</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo ID của các subtask còn thiếu cho mỗi <code>task_id</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Tasks table:
+---------+----------------+
| task_id | subtasks_count |
+---------+----------------+
| 1       | 3              |
| 2       | 2              |
| 3       | 4              |
+---------+----------------+
Executed table:
+---------+------------+
| task_id | subtask_id |
+---------+------------+
| 1       | 2          |
| 3       | 1          |
| 3       | 2          |
| 3       | 3          |
| 3       | 4          |
+---------+------------+
<strong>Đầu ra:</strong>
+---------+------------+
| task_id | subtask_id |
+---------+------------+
| 1       | 1          |
| 1       | 3          |
| 2       | 1          |
| 2       | 2          |
+---------+------------+
<strong>Giải thích:</strong>
Task 1 được chia thành 3 subtask (1, 2, 3). Chỉ subtask 2 được thực thi thành công, nên ta đưa (1, 1) và (1, 3) vào đáp án.
Task 2 được chia thành 2 subtask (1, 2). Không có subtask nào được thực thi thành công, nên ta đưa (2, 1) và (2, 2) vào đáp án.
Task 3 được chia thành 4 subtask (1, 2, 3, 4). Tất cả các subtask đều được thực thi thành công.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tạo bảng đệ quy + Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi task cho biết mình có bao nhiêu subtask; bảng thực thi chỉ liệt kê các subtask đã chạy. Ta cần tìm các cặp $(\textit{task\_id},\textit{subtask\_id})$ còn thiếu.
>
> Một CTE đệ quy đếm lùi từ $\textit{subtasks\_count}$ về $1$, sau đó left join với $\textit{Executed}$ để giữ lại các dòng không có kết quả khớp.

<!-- thinking:end -->

Ta có thể tạo đệ quy một bảng chứa mọi cặp (task cha, task con), rồi dùng left join để tìm các cặp chưa được thực thi.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH RECURSIVE
    T(task_id, subtask_id) AS (
        SELECT
            task_id,
            subtasks_count
        FROM Tasks
        UNION ALL
        SELECT
            task_id,
            subtask_id - 1
        FROM t
        WHERE subtask_id > 1
    )
SELECT
    T.*
FROM
    T
    LEFT JOIN Executed USING (task_id, subtask_id)
WHERE Executed.subtask_id IS NULL;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
