---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3087. Find Trending Hashtags 🔒](https://leetcode.com/problems/find-trending-hashtags)

[中文文档](/solution/3000-3099/3087.Find%20Trending%20Hashtags/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Tweets</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| tweet_id    | int     |
| tweet_date  | date    |
| tweet       | varchar |
+-------------+---------+
tweet_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng của bảng gồm user_id, tweet_id, tweet_date và tweet.
</pre>

<p>Viết truy vấn tìm <strong>top</strong> <code>3</code> <strong>hashtag</strong> thịnh hành nhất trong <strong>tháng 2</strong> năm <code>2024</code>. Mỗi tweet chỉ chứa một hashtag.</p>

<p>Trả về <em>bảng kết quả được sắp xếp theo số lần xuất hiện của hashtag, sau đó theo hashtag, theo thứ tự </em><strong>giảm dần</strong><em>.</em></p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Tweets:</p>

<pre class="example-io">
+---------+----------+----------------------------------------------+------------+
| user_id | tweet_id | tweet                                        | tweet_date |
+---------+----------+----------------------------------------------+------------+
| 135     | 13       | Enjoying a great start to the day! #HappyDay | 2024-02-01 |
| 136     | 14       | Another #HappyDay with good vibes!           | 2024-02-03 |
| 137     | 15       | Productivity peaks! #WorkLife                | 2024-02-04 |
| 138     | 16       | Exploring new tech frontiers. #TechLife      | 2024-02-04 |
| 139     | 17       | Gratitude for today&#39;s moments. #HappyDay     | 2024-02-05 |
| 140     | 18       | Innovation drives us. #TechLife              | 2024-02-07 |
| 141     | 19       | Connecting with nature&#39;s serenity. #Nature   | 2024-02-09 |
+---------+----------+----------------------------------------------+------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-----------+--------------+
| hashtag   | hashtag_count|
+-----------+--------------+
| #HappyDay | 3            |
| #TechLife | 2            |
| #WorkLife | 1            |
+-----------+--------------+

</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>#HappyDay:</strong> Xuất hiện trong các tweet có ID 13, 14 và 17, tổng cộng 3 lần.</li>
    <li><strong>#TechLife:</strong> Xuất hiện trong các tweet có ID 16 và 18, tổng cộng 2 lần.</li>
    <li><strong>#WorkLife:</strong> Xuất hiện trong tweet có ID 15, tổng cộng 1 lần.</li>
</ul>

<p><b>Lưu ý:</b> Bảng kết quả được sắp xếp theo hashtag_count và hashtag theo thứ tự giảm dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trích xuất chuỗi con + Nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số lần xuất hiện của các hashtag trong các tweet tháng 2 năm $2024$, rồi giữ lại ba hashtag đứng đầu theo tần suất và tên. Mỗi tweet đóng góp từ đầu tiên bắt đầu bằng `#`.
>
> Lọc theo tháng, trích xuất tag bằng biểu thức chính quy, thêm `#` vào đầu, rồi lấy ba dòng đầu tiên của $\texttt{value\_counts}$ đã sắp xếp.

<!-- thinking:end -->

Ta có thể truy vấn tất cả tweet trong tháng 2 năm 2024, sử dụng hàm `SUBSTRING_INDEX` để trích xuất hashtag, sau đó dùng các hàm `GROUP BY` và `COUNT` để đếm số lần xuất hiện của từng hashtag. Cuối cùng, ta sắp xếp theo số lần xuất hiện giảm dần và theo hashtag giảm dần, rồi lấy ba hashtag phổ biến nhất.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    CONCAT('#', SUBSTRING_INDEX(SUBSTRING_INDEX(tweet, '#', -1), ' ', 1)) AS hashtag,
    COUNT(1) AS hashtag_count
FROM Tweets
WHERE DATE_FORMAT(tweet_date, '%Y%m') = '202402'
GROUP BY 1
ORDER BY 2 DESC, 1 DESC
LIMIT 3;
```

#### Python3

```python
import pandas as pd


def find_trending_hashtags(tweets: pd.DataFrame) -> pd.DataFrame:
    tweets = tweets[tweets["tweet_date"].dt.strftime("%Y%m") == "202402"]
    tweets["hashtag"] = "#" + tweets["tweet"].str.extract(r"#(\w+)")
    hashtag_counts = tweets["hashtag"].value_counts().reset_index()
    hashtag_counts.columns = ["hashtag", "hashtag_count"]
    hashtag_counts = hashtag_counts.sort_values(
        by=["hashtag_count", "hashtag"], ascending=[False, False]
    )
    top_3_hashtags = hashtag_counts.head(3)
    return top_3_hashtags
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
