---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [1050. Actors and Directors Who Cooperated At Least Three Times](https://leetcode.com/problems/actors-and-directors-who-cooperated-at-least-three-times)

[中文文档](/solution/1000-1099/1050.Actors%20and%20Directors%20Who%20Cooperated%20At%20Least%20Three%20Times/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>ActorDirector</code></p>

<pre>
+-------------+---------+
| Tên cột     | Kiểu    |
+-------------+---------+
| actor_id    | int     |
| director_id | int     |
| timestamp   | int     |
+-------------+---------+
timestamp là khóa chính (cột có giá trị duy nhất) của bảng này.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để tìm tất cả các cặp <code>(actor_id, director_id)</code> mà diễn viên đã hợp tác với đạo diễn ít nhất ba lần.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng ActorDirector:
+-------------+-------------+-------------+
| actor_id    | director_id | timestamp   |
+-------------+-------------+-------------+
| 1           | 1           | 0           |
| 1           | 1           | 1           |
| 1           | 1           | 2           |
| 1           | 2           | 3           |
| 1           | 2           | 4           |
| 2           | 1           | 5           |
| 2           | 1           | 6           |
+-------------+-------------+-------------+
<strong>Đầu ra:</strong> 
+-------------+-------------+
| actor_id    | director_id |
+-------------+-------------+
| 1           | 1           |
+-------------+-------------+
<strong>Giải thích:</strong> Cặp duy nhất là (1, 1), với đúng 3 lần hợp tác.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Group By + Having

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần biết mỗi cặp diễn viên–đạo diễn đã hợp tác bao nhiêu lần. Group theo cả hai id sẽ tạo ra một nhóm có số dòng bằng số lần hợp tác.
>
> `GROUP BY actor_id, director_id` và `HAVING COUNT(1) >= 3` giữ lại các cặp có ít nhất ba dòng.
>
> Chỉ cần một phép aggregation, không cần self-join.

<!-- thinking:end -->

Ta có thể dùng câu lệnh `GROUP BY` để nhóm dữ liệu theo các trường `actor_id` và `director_id`, sau đó dùng `HAVING` để lọc các cặp `actor_id` và `director_id` xuất hiện ít nhất ba lần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT actor_id, director_id
FROM ActorDirector
GROUP BY 1, 2
HAVING COUNT(1) >= 3;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
