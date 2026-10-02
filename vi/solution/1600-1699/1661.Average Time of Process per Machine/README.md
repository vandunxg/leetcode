---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1661. Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine)

[中文文档](/solution/1600-1699/1661.Average%20Time%20of%20Process%20per%20Machine/README.md)

## Mô tả

<!-- description:start -->

<p>Table: <code>Activity</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
Bảng này lưu các hoạt động của người dùng trên website của nhà máy.
(machine_id, process_id, activity_type) is the primary key (combination of columns with unique values) of this table.
machine_id là ID của một máy.
process_id là ID của một process đang chạy trên máy có ID machine_id.
activity_type là một ENUM (category) có kiểu (&#39;start&#39;, &#39;end&#39;).
timestamp là số thực biểu diễn thời gian hiện tại theo giây.
&#39;start&#39; nghĩa là máy bắt đầu process tại timestamp đã cho, còn &#39;end&#39; nghĩa là máy kết thúc process tại timestamp đã cho.
The `start` timestamp will always be less than or equal to the `end` timestamp for every `(machine_id, process_id)` pair.
It is guaranteed that each (machine_id, process_id) pair has a &#39;start&#39; and &#39;end&#39; timestamp.
</pre>

<p>&nbsp;</p>

<p>Website của nhà máy có nhiều máy, mỗi máy chạy <strong>cùng số lượng process</strong>. Hãy viết lời giải để tìm <strong>thời gian trung bình</strong> mỗi máy cần để hoàn thành một process.</p>

<p>Thời gian hoàn thành một process bằng <code>&#39;end&#39; timestamp</code> trừ <code>&#39;start&#39; timestamp</code>. Thời gian trung bình bằng tổng thời gian hoàn thành mọi process trên máy chia cho số process đã chạy.</p>

<p>Bảng kết quả cần có <code>machine_id</code> cùng <strong>thời gian trung bình</strong> dưới tên <code>processing_time</code>, được <strong>làm tròn đến 3 chữ số thập phân</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>The result format is in the following example.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong>
Activity table:
+------------+------------+---------------+-----------+
| machine_id | process_id | activity_type | timestamp |
+------------+------------+---------------+-----------+
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |
+------------+------------+---------------+-----------+
<strong>Output:</strong>
+------------+-----------------+
| machine_id | processing_time |
+------------+-----------------+
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |
+------------+-----------------+
<strong>Explanation:</strong>
Có 3 máy, mỗi máy chạy 2 process.
Thời gian trung bình của máy 0 là ((1.520 - 0.712) + (4.120 - 3.140)) / 2 = 0.894
Thời gian trung bình của máy 1 là ((1.550 - 0.550) + (1.420 - 0.430)) / 2 = 0.995
Thời gian trung bình của máy 2 là ((4.512 - 4.100) + (5.000 - 2.500)) / 2 = 1.456
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Grouping và Aggregation

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi process trên một máy có cặp start/end; thời lượng trung bình là trung bình của $(\textit{end}-\textit{start})$. Khi group theo $\texttt{machine\_id}$, đổi dấu các start rồi lấy trung bình sẽ cho một nửa hiệu cần tìm, vì vậy ta nhân với $2$.
>
> $\texttt{CASE WHEN}$ cung cấp dấu; sau đó $\texttt{AVG}$ được $\texttt{ROUND}$ đến ba chữ số thập phân.

<!-- thinking:end -->

Ta có thể group theo `machine_id` và dùng hàm `AVG` để tính thời gian trung bình của mọi process trên mỗi máy. Vì mỗi process trên máy có một cặp timestamp start và end, thời gian của process được tính bằng timestamp `end` trừ timestamp `start`. Do đó, ta có thể dùng hàm `CASE WHEN` hoặc `IF` để tính thời gian của từng process, rồi dùng `AVG` để tính thời gian trung bình của mọi process trên mỗi máy.

Lưu ý mỗi máy có $2$ process, nên ta cần nhân giá trị trung bình đã tính với $2$.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    machine_id,
    ROUND(
        AVG(
            CASE
                WHEN activity_type = 'start' THEN -timestamp
                ELSE timestamp
            END
        ) * 2,
        3
    ) AS processing_time
FROM Activity
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 rẽ nhánh bằng $\texttt{CASE}$. Có thể dùng mẹo dấu tương tự với $\texttt{IF}(\textit{start},-1,1)*\textit{timestamp}$, biểu thức ngắn hơn nhưng cùng ý nghĩa.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    machine_id,
    ROUND(AVG(IF(activity_type = 'start', -1, 1) * timestamp) * 2, 3) AS processing_time
FROM Activity
GROUP BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
