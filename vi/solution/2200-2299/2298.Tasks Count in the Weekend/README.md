---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2298. Tasks Count in the Weekend 🔒](https://leetcode.com/problems/tasks-count-in-the-weekend)

[中文文档](/solution/2200-2299/2298.Tasks%20Count%20in%20the%20Weekend/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Tasks</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| task_id     | int  |
| assignee_id | int  |
| submit_date | date |
+-------------+------+
task_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này chứa ID của một task, ID của người được giao task và ngày gửi task.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo:</p>

<ul>
    <li>số lượng task được gửi trong cuối tuần (thứ Bảy, Chủ nhật) với tên là <code>weekend_cnt</code>, và</li>
    <li>số lượng task được gửi trong các ngày làm việc với tên là <code>working_cnt</code>.</li>
</ul>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Tasks table:
+---------+-------------+-------------+
| task_id | assignee_id | submit_date |
+---------+-------------+-------------+
| 1       | 1           | 2022-06-13  |
| 2       | 6           | 2022-06-14  |
| 3       | 6           | 2022-06-15  |
| 4       | 3           | 2022-06-18  |
| 5       | 5           | 2022-06-19  |
| 6       | 7           | 2022-06-19  |
+---------+-------------+-------------+
<strong>Đầu ra:</strong>
+-------------+-------------+
| weekend_cnt | working_cnt |
+-------------+-------------+
| 3           | 3           |
+-------------+-------------+
<strong>Giải thích:</strong>
Task 1 được gửi vào thứ Hai.
Task 2 được gửi vào thứ Ba.
Task 3 được gửi vào thứ Tư.
Task 4 được gửi vào thứ Bảy.
Task 5 được gửi vào Chủ nhật.
Task 6 được gửi vào Chủ nhật.
Có 3 task được gửi trong cuối tuần.
Có 3 task được gửi trong các ngày làm việc.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số task được gửi vào cuối tuần và số task được gửi vào các ngày trong tuần. $\textit{WEEKDAY}$ ánh xạ từ thứ Hai đến Chủ nhật thành các số từ $0$ đến $6$, vì vậy thứ Bảy và Chủ nhật lần lượt là $5$ và $6$.
>
> Một phép tổng hợp duy nhất tính tổng của điều kiện $\textit{WEEKDAY}(\textit{submit\_date}) \in (5,6)$ để có số task cuối tuần, và tổng của phủ định điều kiện đó để có số task trong các ngày làm việc.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    SUM(WEEKDAY(submit_date) IN (5, 6)) AS weekend_cnt,
    SUM(WEEKDAY(submit_date) NOT IN (5, 6)) AS working_cnt
FROM Tasks;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
