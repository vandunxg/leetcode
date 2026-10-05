---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3832. Find Users with Persistent Behavior Patterns](https://leetcode.com/problems/find-users-with-persistent-behavior-patterns)

[中文文档](/solution/3800-3899/3832.Find%20Users%20with%20Persistent%20Behavior%20Patterns/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>activity</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| user_id      | int     |
| action_date  | date    |
| action       | varchar |
+--------------+---------+
(user_id, action_date, action) is the primary key (unique value) for this table.
Each row represents a user performing a specific action on a given date.
</pre>

<p>Hãy viết lời giải để xác định <strong>những người dùng có hành vi ổn định</strong> dựa trên định nghĩa sau:</p>

<ul>
	<li>Một người dùng được xem là <strong>có hành vi ổn định</strong> nếu tồn tại một chuỗi <strong>ít nhất </strong><code>5</code><strong> ngày liên tiếp</strong> sao cho:

    <ul>
    	<li>Người dùng thực hiện <strong>đúng một hành động mỗi ngày</strong> trong khoảng thời gian đó.</li>
    	<li><strong>Hành động giống nhau</strong> trong tất cả những ngày liên tiếp đó.</li>
    </ul>
    </li>
    <li>Nếu một người dùng có nhiều chuỗi thỏa mãn, chỉ xét chuỗi có <strong>độ dài lớn nhất</strong>.</li>

</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>streak_length</code> <em>theo thứ tự <strong>giảm dần</strong></em>,<em> sau đó theo </em><code>user_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng activity:</p>

<pre class="example-io">
+---------+-------------+--------+
| user_id | action_date | action |
+---------+-------------+--------+
| 1       | 2024-01-01  | login  |
| 1       | 2024-01-02  | login  |
| 1       | 2024-01-03  | login  |
| 1       | 2024-01-04  | login  |
| 1       | 2024-01-05  | login  |
| 1       | 2024-01-06  | logout |
| 2       | 2024-01-01  | click  |
| 2       | 2024-01-02  | click  |
| 2       | 2024-01-03  | click  |
| 2       | 2024-01-04  | click  |
| 3       | 2024-01-01  | view   |
| 3       | 2024-01-02  | view   |
| 3       | 2024-01-03  | view   |
| 3       | 2024-01-04  | view   |
| 3       | 2024-01-05  | view   |
| 3       | 2024-01-06  | view   |
| 3       | 2024-01-07  | view   |
+---------+-------------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+--------+---------------+------------+------------+
| user_id | action | streak_length | start_date | end_date   |
+---------+--------+---------------+------------+------------+
| 3       | view   | 7             | 2024-01-01 | 2024-01-07 |
| 1       | login  | 5             | 2024-01-01 | 2024-01-05 |
+---------+--------+---------------+------------+------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Người dùng 1</strong>:

    <ul>
    	<li>Thực hiện <code>login</code> từ 2024-01-01 đến 2024-01-05 vào những ngày liên tiếp</li>
    	<li>Mỗi ngày có đúng một hành động và hành động đó giống nhau</li>
    	<li>Độ dài chuỗi = 5 (đáp ứng yêu cầu tối thiểu)</li>
    	<li>Hành động thay đổi vào ngày 2024-01-06, kết thúc chuỗi</li>
    </ul>
    </li>
    <li><strong>Người dùng 2</strong>:
    <ul>
    	<li>Thực hiện <code>click</code> trong chỉ 4 ngày liên tiếp</li>
    	<li>Không đáp ứng độ dài chuỗi tối thiểu là 5</li>
    	<li>Bị loại khỏi kết quả</li>
    </ul>
    </li>
    <li><strong>Người dùng 3</strong>:
    <ul>
    	<li>Thực hiện <code>view</code> trong 7 ngày liên tiếp</li>
    	<li>Đây là chuỗi hợp lệ dài nhất của người dùng này</li>
    	<li>Được đưa vào kết quả</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo streak_length giảm dần, sau đó theo user_id tăng dần</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lọc + Nhóm + Tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Người dùng có hành vi ổn định phải có ít nhất năm ngày liên tiếp với đúng một hành động, và hành động đó luôn giống nhau. Có thể tồn tại nhiều chuỗi; chỉ giữ lại chuỗi dài nhất.
>
> Những ngày có nhiều hơn một hành động phải bị loại bỏ, nếu không tính liên tiếp sẽ không còn đúng.
>
> Sau khi sắp xếp từng $(\textit{user\_id},\textit{action})$ theo ngày, ngày trừ đi số thứ tự dòng sẽ nhóm các ngày liên tiếp thành một $\textit{grp}$.
>
> Tổng hợp độ dài, giữ lại những chuỗi có độ dài $\ge 5$, xếp hạng một chuỗi cho mỗi người dùng, rồi sắp xếp theo độ dài và id.

<!-- thinking:end -->

Trước tiên, chúng ta cần lọc những ngày mà người dùng chỉ thực hiện một hành động, sau đó xác định các khoảng thời gian liên tiếp trong những ngày này, cuối cùng tổng hợp các khoảng thời gian để tính độ dài chuỗi và lọc những bản ghi đáp ứng các tiêu chí.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    daily_counts AS (
        -- Step 1: Filter user dates with exactly one record per day (meeting the requirement of "exactly one action per day")
        SELECT
            user_id,
            action_date,
            action,
            COUNT(*) OVER (PARTITION BY user_id, action_date) AS cnt
        FROM activity
    ),
    filtered_activity AS (
        -- Step 2: Filter out data with multiple actions on the same day
        SELECT user_id, action_date, action
        FROM daily_counts
        WHERE cnt = 1
    ),
    streak_groups AS (
        -- Step 3: Group consecutive dates using the method of subtracting row number from date
        SELECT
            user_id,
            action,
            action_date,
            DATE_SUB(
                action_date,
                INTERVAL ROW_NUMBER() OVER (
                    PARTITION BY user_id, action
                    ORDER BY action_date
                ) DAY
            ) AS grp
        FROM filtered_activity
    ),
    streak_summary AS (
        -- Step 4: Calculate the length of each consecutive segment and only keep records with length >= 5
        SELECT
            user_id,
            action,
            COUNT(*) AS streak_length,
            MIN(action_date) AS start_date,
            MAX(action_date) AS end_date,
            -- Sort different streaks for each user to facilitate getting the maximum value later
            ROW_NUMBER() OVER (
                PARTITION BY user_id
                ORDER BY COUNT(*) DESC
            ) AS rnk
        FROM streak_groups
        GROUP BY user_id, action, grp
        HAVING streak_length >= 5
    )
-- Step 5: Extract the longest record for each qualified user and sort
SELECT user_id, action, streak_length, start_date, end_date
FROM streak_summary
WHERE rnk = 1
ORDER BY streak_length DESC, user_id ASC;
```

#### Pandas

```python
import pandas as pd

def find_behaviorally_stable_users(activity: pd.DataFrame) -> pd.DataFrame:
    activity['action_date'] = pd.to_datetime(activity['action_date'])

    # Filter users with only a single action per day
    df = activity.assign(cnt=activity.groupby(['user_id', 'action_date'])['action'].transform('count'))
    df = df[df['cnt'] == 1].sort_values(['user_id', 'action', 'action_date'])

    # Identify consecutive intervals
    df['rn'] = df.groupby(['user_id', 'action'])['action_date'].rank(method='first')
    df['grp'] = df['action_date'] - pd.to_timedelta(df['rn'], unit='D')

    # Aggregate streaks
    streaks = df.groupby(['user_id', 'action', 'grp']).agg(
        streak_length=('action_date', 'count'),
        start_date=('action_date', 'min'),
        end_date=('action_date', 'max')
    ).reset_index()

    # Filter and get the longest streak for each user
    res = streaks[streaks['streak_length'] >= 5].sort_values(
        ['streak_length', 'user_id'], ascending=[False, True]
    )

    return res.groupby('user_id').head(1)[['user_id', 'action', 'streak_length', 'start_date', 'end_date']]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
