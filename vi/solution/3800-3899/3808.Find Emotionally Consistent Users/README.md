---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3808. Find Emotionally Consistent Users](https://leetcode.com/problems/find-emotionally-consistent-users)

[中文文档](/solution/3800-3899/3808.Find%20Emotionally%20Consistent%20Users/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>reactions</code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| user_id      | int     |
| content_id   | int     |
| reaction     | varchar |
+--------------+---------+
(user_id, content_id) is the primary key (unique value) for this table.
Each row represents a reaction given by a user to a piece of content.
</pre>

<p>Hãy viết lời giải để xác định <strong>những người dùng có cảm xúc nhất quán</strong> dựa trên các yêu cầu sau:</p>

<ul>
	<li>Với mỗi người dùng, đếm tổng số reaction mà họ đã thực hiện.</li>
	<li>Chỉ bao gồm những người dùng đã reaction cho <strong>ít nhất </strong><code>5</code><strong> nội dung khác nhau</strong>.</li>
	<li>Một người dùng được xem là <strong>có cảm xúc nhất quán</strong> nếu <strong>ít nhất </strong><code>60%</code> số reaction của họ thuộc về <strong>cùng một loại</strong>.</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>reaction_ratio</code> <em>theo thứ tự <strong>giảm dần</strong>, sau đó theo</em> <code>user_id</code> <em>theo thứ tự <strong>tăng dần</strong></em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><code>reaction_ratio</code>&nbsp;phải được làm tròn đến <code>2</code> chữ số thập phân</li>
</ul>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng reactions:</p>

<pre class="example-io">
+---------+------------+----------+
| user_id | content_id | reaction |
+---------+------------+----------+
| 1       | 101        | like     |
| 1       | 102        | like     |
| 1       | 103        | like     |
| 1       | 104        | wow      |
| 1       | 105        | like     |
| 2       | 201        | like     |
| 2       | 202        | wow      |
| 2       | 203        | sad      |
| 2       | 204        | like     |
| 2       | 205        | wow      |
| 3       | 301        | love     |
| 3       | 302        | love     |
| 3       | 303        | love     |
| 3       | 304        | love     |
| 3       | 305        | love     |
+---------+------------+----------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+-------------------+----------------+
| user_id | dominant_reaction | reaction_ratio |
+---------+-------------------+----------------+
| 3       | love              | 1.00           |
| 1       | like              | 0.80           |
+---------+-------------------+----------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Người dùng 1</strong>:

    <ul>
    	<li>Tổng số reaction = 5</li>
    	<li>like xuất hiện 4 lần</li>
    	<li>reaction_ratio = 4 / 5 = 0.80</li>
    	<li>Đáp ứng yêu cầu nhất quán 60%</li>
    </ul>
    </li>
    <li><strong>Người dùng 2</strong>:
    <ul>
    	<li>Tổng số reaction = 5</li>
    	<li>Reaction xuất hiện nhiều nhất chỉ 2 lần</li>
    	<li>reaction_ratio = 2 / 5 = 0.40</li>
    	<li>Không đáp ứng yêu cầu nhất quán</li>
    </ul>
    </li>
    <li><strong>Người dùng 3</strong>:
    <ul>
    	<li>Tổng số reaction = 5</li>
    	<li>&#39;love&#39; xuất hiện 5 lần</li>
    	<li>reaction_ratio = 5 / 5 = 1.00</li>
    	<li>Đáp ứng yêu cầu nhất quán</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo reaction_ratio giảm dần, sau đó theo user_id tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thống kê theo nhóm + truy vấn JOIN

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần đếm reaction theo từng người dùng, tìm loại reaction chiếm ưu thế và tỷ lệ của nó, đồng thời loại những người dùng có ít hơn $5$ reaction hoặc tỷ lệ dưới $0.60$. Việc quét lại bảng gốc nhiều lần sẽ tính lại cùng một kết quả tổng hợp.
>
> Các loại reaction của một người dùng phải được dùng cho cả việc tìm số lượng lớn nhất và tổng số reaction, sau đó cần tìm loại đạt số lượng lớn nhất đó.
>
> Nhóm theo $(\textit{user\_id},\textit{reaction})$ vào $t$, sau đó tổng hợp $t$ theo người dùng vào $s$ với số lượng lớn nhất và điều kiện lọc tỷ lệ.
>
> JOIN $s$ trở lại $t$ theo người dùng, giữ lại các dòng có số lượng bằng giá trị lớn nhất, rồi sắp xếp theo tỷ lệ và id. Việc nhóm trước khi JOIN giúp tránh phải lọc các dòng chi tiết nhiều lần.

<!-- thinking:end -->

Trước tiên, chúng ta đếm số lần xuất hiện của mỗi reaction đối với từng người dùng và lưu kết quả vào bảng tạm $t$. Sau đó, dựa trên bảng tạm $t$, chúng ta tính số reaction lớn nhất và tổng số reaction của mỗi người dùng, tính reaction ratio, rồi lọc những người dùng thỏa mãn các điều kiện và lưu vào bảng tạm $s$. Cuối cùng, chúng ta JOIN hai bảng tạm $s$ và $t$ để tìm reaction chiếm ưu thế của mỗi người dùng, rồi sắp xếp kết quả theo yêu cầu.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    t AS (
        SELECT
            user_id,
            reaction,
            COUNT(1) cnt
        FROM reactions
        GROUP BY 1, 2
    ),
    s AS (
        SELECT
            user_id,
            MAX(cnt) mx_cnt,
            ROUND(MAX(cnt) / SUM(cnt), 2) reaction_ratio
        FROM t
        GROUP BY 1
        HAVING reaction_ratio >= 0.60 AND SUM(cnt) >= 5
    )
SELECT user_id, reaction dominant_reaction, reaction_ratio
FROM
    s
    JOIN t USING (user_id)
WHERE cnt = mx_cnt
ORDER BY 3 DESC, 1;
```

#### Pandas

```python
import pandas as pd
from decimal import Decimal, ROUND_HALF_UP


def find_emotionally_consistent_users(reactions: pd.DataFrame) -> pd.DataFrame:
    t = reactions.groupby(["user_id", "reaction"]).size().reset_index(name="cnt")

    s = (
        t.groupby("user_id")
        .agg(mx_cnt=("cnt", "max"), total_cnt=("cnt", "sum"))
        .reset_index()
    )

    s["reaction_ratio"] = (
        s["mx_cnt"]
        .div(s["total_cnt"])
        .apply(
            lambda x: float(
                Decimal(str(x)).quantize(Decimal("0.00"), rounding=ROUND_HALF_UP)
            )
        )
    )

    s = s[(s["reaction_ratio"] >= 0.60) & (s["total_cnt"] >= 5)]

    merged = pd.merge(
        s[["user_id", "mx_cnt", "reaction_ratio"]],
        t,
        left_on=["user_id", "mx_cnt"],
        right_on=["user_id", "cnt"],
    )

    result = (
        merged[["user_id", "reaction", "reaction_ratio"]]
        .rename(columns={"reaction": "dominant_reaction"})
        .sort_values(by=["reaction_ratio", "user_id"], ascending=[False, True])
        .reset_index(drop=True)
    )

    return result
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
