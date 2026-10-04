---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3089. Find Bursty Behavior 🔒](https://leetcode.com/problems/find-bursty-behavior)

[中文文档](/solution/3000-3099/3089.Find%20Bursty%20Behavior/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Posts</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| post_id     | int     |
| user_id     | int     |
| post_date   | date    |
+-------------+---------+
post_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này chứa post_id, user_id và post_date.
</pre>

<p>Viết lời giải để tìm những người dùng thể hiện <strong>hành vi đăng bài dồn dập</strong> trong cách đăng bài của họ trong tháng 2 năm <code>2024</code>. <strong>Hành vi đăng bài dồn dập</strong> được định nghĩa là <strong>bất kỳ</strong> khoảng thời gian <strong>7</strong> ngày <strong>liên tiếp</strong> nào mà tần suất đăng bài của người dùng <strong>ít nhất gấp đôi</strong> tần suất đăng bài <strong>trung bình</strong> mỗi tuần của họ trong tháng 2 năm <code>2024</code>.</p>

<p><strong>Lưu ý:</strong> Chỉ phân tích các ngày từ ngày <code>1</code> đến ngày <code>28</code> tháng 2, nghĩa là coi tháng 2 có đúng <code>4</code> tuần.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo </em><code>user_id</code><em> theo thứ tự </em><strong>tăng dần</strong><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Posts:</p>

<pre class="example-io">
+---------+---------+------------+
| post_id | user_id | post_date  |
+---------+---------+------------+
| 1       | 1       | 2024-02-27 |
| 2       | 5       | 2024-02-06 |
| 3       | 3       | 2024-02-25 |
| 4       | 3       | 2024-02-14 |
| 5       | 3       | 2024-02-06 |
| 6       | 2       | 2024-02-25 |
+---------+---------+------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+----------------+------------------+
| user_id | max_7day_posts | avg_weekly_posts |
+---------+----------------+------------------+
| 1       | 1              | 0.2500           |
| 2       | 1              | 0.2500           |
| 5       | 1              | 0.2500           |
+---------+----------------+------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>Người dùng 1:</strong> Chỉ đăng 1 bài trong tháng 2, nên trung bình có 0.25 bài mỗi tuần và tối đa 1 bài trong bất kỳ khoảng thời gian 7 ngày nào.</li>
	<li><strong>Người dùng 2:</strong> Cũng chỉ đăng 1 bài, có cùng mức trung bình và tần suất đăng bài tối đa trong 7 ngày như Người dùng 1.</li>
	<li><strong>Người dùng 5:</strong> Giống Người dùng 1 và 2, Người dùng 5 chỉ đăng 1 bài trong cả tháng 2, nên có cùng các chỉ số trung bình và đăng bài tối đa trong 7 ngày.</li>
	<li><strong>Người dùng 3:</strong> Mặc dù Người dùng 3 đăng nhiều bài hơn những người khác (3 bài), họ không đạt gấp đôi số bài trung bình mỗi tuần trong khoảng thời gian 7 ngày liên tiếp, nên không xuất hiện trong kết quả.</li>
</ul>

<p><b>Lưu ý:</b> Bảng kết quả được sắp xếp theo user_id theo thứ tự tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Self-Join + Group Count

<!-- thinking:start -->

> **Tư duy**
>
> Một đợt đăng bài dồn dập xảy ra khi có một khoảng thời gian $7$ ngày chứa số bài đăng ít nhất gấp đôi số bài trung bình mỗi tuần của người dùng trong tháng 2 năm $2024$. Cả hai đại lượng này đều được tính từ cùng một bảng bài đăng.
>
> Self-join theo user với các ngày nằm trong $[d,d+6]$ giúp đếm số bài trong mỗi khoảng thời gian; số bài đăng trong tháng 2 chia cho $4$ là số bài trung bình mỗi tuần. Sau đó, nối hai kết quả và giữ lại khoảng thời gian có số bài lớn nhất.
>
> Đoạn code xây dựng self-join theo ngày, tính giá trị trung bình trong tháng 2, inner join hai kết quả, lọc theo ngưỡng và thực hiện phép gộp.

<!-- thinking:end -->

Ta có thể dùng self-join để nối bảng `Posts` với chính nó. Điều kiện nối là `p1.user_id = p2.user_id` và `p2.post_date` nằm trong khoảng từ `p1.post_date` đến 6 ngày sau `p1.post_date`. Sau đó, nhóm kết quả nối theo `p1.user_id` và `p1.post_id` để đếm số bài đăng của mỗi người dùng trong vòng 7 ngày tính từ mỗi ngày. Lưu kết quả này vào bảng `P`.

Tiếp theo, ta đếm số bài đăng trung bình mỗi tuần của từng người dùng trong tháng 2 năm 2024 và lưu vào bảng `T`. Cụ thể, cần tìm các bản ghi có `post_date` nằm trong khoảng từ `2024-02-01` đến `2024-02-28`, nhóm các bản ghi theo `user_id`, đếm số bài đăng của mỗi người dùng, rồi chia cho `4` để tính số bài đăng trung bình mỗi tuần. Lưu kết quả này vào bảng `T`.

Cuối cùng, nối các bảng `P` và `T` với điều kiện `P.user_id = T.user_id`, sau đó nhóm theo `user_id` để tìm số bài đăng lớn nhất trong 7 ngày của mỗi người dùng. Tiếp đó, lọc các bản ghi thỏa điều kiện `max_7day_posts >= avg_weekly_posts * 2` để thu được kết quả. Lưu ý rằng cần sắp xếp theo `user_id` theo thứ tự tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT p1.user_id AS user_id, COUNT(1) AS cnt
        FROM
            Posts AS p1
            JOIN Posts AS p2
                ON p1.user_id = p2.user_id
                AND p2.post_date BETWEEN p1.post_date AND DATE_ADD(p1.post_date, INTERVAL 6 DAY)
        GROUP BY p1.user_id, p1.post_id
    ),
    T AS (
        SELECT user_id, COUNT(1) / 4 AS avg_weekly_posts
        FROM Posts
        WHERE post_date BETWEEN '2024-02-01' AND '2024-02-28'
        GROUP BY 1
    )
SELECT user_id, MAX(cnt) AS max_7day_posts, avg_weekly_posts
FROM
    P
    JOIN T USING (user_id)
GROUP BY 1
HAVING max_7day_posts >= avg_weekly_posts * 2
ORDER BY 1;
```

#### Python3

```python
import pandas as pd


def find_bursty_behavior(posts: pd.DataFrame) -> pd.DataFrame:
    # Subquery P
    p1 = pd.merge(posts, posts, on="user_id", suffixes=("_1", "_2"))
    p1 = p1[
        p1["post_date_2"].between(
            p1["post_date_1"], p1["post_date_1"] + pd.Timedelta(days=6)
        )
    ]
    p1 = p1.groupby(["user_id", "post_id_1"]).size().reset_index(name="cnt")

    # Subquery T
    t = posts[
        (posts["post_date"] >= "2024-02-01") & (posts["post_date"] <= "2024-02-28")
    ]
    t = t.groupby("user_id").size().div(4).reset_index(name="avg_weekly_posts")

    # Joining P and T
    merged_df = pd.merge(p1, t, on="user_id", how="inner")

    # Filtering
    filtered_df = merged_df[merged_df["cnt"] >= merged_df["avg_weekly_posts"] * 2]

    # Aggregating
    result_df = (
        filtered_df.groupby("user_id")
        .agg({"cnt": "max", "avg_weekly_posts": "first"})
        .reset_index()
    )
    result_df.columns = ["user_id", "max_7day_posts", "avg_weekly_posts"]

    # Sorting
    result_df.sort_values(by="user_id", inplace=True)

    return result_df
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
