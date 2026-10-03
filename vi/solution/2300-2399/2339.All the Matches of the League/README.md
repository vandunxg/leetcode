---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2339. All the Matches of the League 🔒](https://leetcode.com/problems/all-the-matches-of-the-league)

[中文文档](/solution/2300-2399/2339.All%20the%20Matches%20of%20the%20League/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Teams</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| team_name   | varchar |
+-------------+---------+
team_name là cột chứa các giá trị duy nhất trong bảng này.
Mỗi hàng trong bảng này hiển thị tên của một đội.
</pre>

<p>&nbsp;</p>

<p>Viết lời giải để báo cáo tất cả các trận đấu có thể có trong giải đấu. Lưu ý rằng mọi hai đội sẽ thi đấu với nhau hai trận, trong đó một đội làm <code>home_team</code> một lần và lần còn lại làm <code>away_team</code>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Teams table:
+-------------+
| team_name   |
+-------------+
| Leetcode FC |
| Ahly SC     |
| Real Madrid |
+-------------+
<strong>Đầu ra:</strong>
+-------------+-------------+
| home_team   | away_team   |
+-------------+-------------+
| Real Madrid | Leetcode FC |
| Real Madrid | Ahly SC     |
| Leetcode FC | Real Madrid |
| Leetcode FC | Ahly SC     |
| Ahly SC     | Real Madrid |
| Ahly SC     | Leetcode FC |
+-------------+-------------+
<strong>Giải thích:</strong> Tất cả các trận đấu của giải đấu được liệt kê trong bảng.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Giải đấu cần mọi cặp có thứ tự gồm hai đội khác nhau. Phép self-join trên $Teams$ sau khi loại các tên trùng nhau sẽ liệt kê các cặp này.
>
> Ta lấy tích Descartes của $t1$ và $t2$ với $t1.team\_name \ne t2.team\_name$, trong đó lần lượt là đội chủ nhà và đội khách.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT t1.team_name AS home_team, t2.team_name AS away_team
FROM
    Teams AS t1
    JOIN Teams AS t2
WHERE t1.team_name != t2.team_name;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
