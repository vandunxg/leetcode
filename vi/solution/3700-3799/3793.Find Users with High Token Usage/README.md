---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3793. Find Users with High Token Usage](https://leetcode.com/problems/find-users-with-high-token-usage)

[Tài liệu tiếng Trung](/solution/3700-3799/3793.Find%20Users%20with%20High%20Token%20Usage/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>prompts</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| prompt      | varchar |
| tokens      | int     |
+-------------+---------+
(user_id, prompt) is the primary key (unique value) for this table.
Each row represents a prompt submitted by a user to an AI system along with the number of tokens consumed.
</pre>

<p>Hãy viết lời giải để phân tích <strong>mẫu sử dụng prompt AI</strong> dựa trên các yêu cầu sau:</p>

<ul>
	<li>Với mỗi người dùng, tính <strong>tổng số prompt</strong> mà họ đã gửi.</li>
	<li>Với mỗi người dùng, tính <strong>số token trung bình được sử dụng cho mỗi prompt </strong>(làm tròn đến <code>2</code> chữ số thập phân).</li>
	<li>Chỉ bao gồm những người dùng đã gửi <strong>ít nhất </strong><code>3</code><strong> prompt</strong>.</li>
	<li>Chỉ bao gồm những người dùng đã gửi <strong>ít nhất một prompt</strong> có <code>tokens</code> <strong>lớn hơn</strong> số token trung bình của chính họ.</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo <strong>số token trung bình</strong> theo thứ tự <strong>giảm dần</strong>, sau đó theo <code>user_id</code> theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng prompts:</p>

<pre class="example-io">
+---------+--------------------------+--------+
| user_id | prompt                   | tokens |
+---------+--------------------------+--------+
| 1       | Write a blog outline     | 120    |
| 1       | Generate SQL query       | 80     |
| 1       | Summarize an article     | 200    |
| 2       | Create resume bullet     | 60     |
| 2       | Improve LinkedIn bio     | 70     |
| 3       | Explain neural networks  | 300    |
| 3       | Generate interview Q&amp;A   | 250    |
| 3       | Write cover letter       | 180    |
| 3       | Optimize Python code     | 220    |
+---------+--------------------------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+---------------+------------+
| user_id | prompt_count  | avg_tokens |
+---------+---------------+------------+
| 3       | 4             | 237.5      |
| 1       | 3             | 133.33     |
+---------+---------------+------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Người dùng 1</strong>:

    <ul>
    	<li>Tổng số prompt = 3</li>
    	<li>Số token trung bình = (120 + 80 + 200) / 3 = 133.33</li>
    	<li>Có một prompt sử dụng 200 token, lớn hơn mức trung bình</li>
    	<li>Được đưa vào kết quả</li>
    </ul>
    </li>
    <li><strong>Người dùng 2</strong>:
    <ul>
    	<li>Tổng số prompt = 2 (ít hơn mức tối thiểu yêu cầu)</li>
    	<li>Bị loại khỏi kết quả</li>
    </ul>
    </li>
    <li><strong>Người dùng 3</strong>:
    <ul>
    	<li>Tổng số prompt = 4</li>
    	<li>Số token trung bình = (300 + 250 + 180 + 220) / 4 = 237.5</li>
    	<li>Có các prompt sử dụng 300 và 250 token, đều lớn hơn mức trung bình</li>
    	<li>Được đưa vào kết quả</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo avg_tokens giảm dần, sau đó theo user_id tăng dần</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm và tính thống kê

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi người dùng, chúng ta cần số prompt, số token trung bình và kiểm tra xem có prompt nào vượt mức trung bình hay không. Nhóm ba thống kê này theo `user_id`, giữ lại những người dùng có ít nhất ba prompt và $\textit{max}>\textit{mean}$, sau đó sắp xếp theo số token trung bình và user id.

<!-- thinking:end -->

Trước tiên, chúng ta nhóm các prompt theo `user_id` và tính tổng số prompt `prompt_count`, số token trung bình `avg_tokens` và số token lớn nhất `max_tokens` cho mỗi người dùng. Sau đó, chúng ta lọc những người dùng thỏa mãn các điều kiện, tức là đã gửi ít nhất 3 prompt và có ít nhất một prompt sử dụng số token lớn hơn mức trung bình của họ. Cuối cùng, chúng ta sắp xếp kết quả theo `avg_tokens` giảm dần và theo `user_id` tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    user_id,
    COUNT(1) AS prompt_count,
    ROUND(AVG(tokens), 2) AS avg_tokens
FROM prompts
GROUP BY user_id
HAVING prompt_count >= 3 AND MAX(tokens) > avg_tokens
ORDER BY avg_tokens DESC, user_id;
```

#### Pandas

```python
import pandas as pd


def find_users_with_high_tokens(prompts: pd.DataFrame) -> pd.DataFrame:
    df = prompts.groupby("user_id", as_index=False).agg(
        prompt_count=("user_id", "size"),
        avg_tokens=("tokens", "mean"),
        max_tokens=("tokens", "max"),
    )

    df["avg_tokens"] = df["avg_tokens"].round(2)

    df = df[(df["prompt_count"] >= 3) & (df["max_tokens"] > df["avg_tokens"])]

    df = (
        df.sort_values(["avg_tokens", "user_id"], ascending=[False, True])
        .loc[:, ["user_id", "prompt_count", "avg_tokens"]]
        .reset_index(drop=True)
    )

    return df
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
