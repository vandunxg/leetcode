---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3103. Find Trending Hashtags II 🔒](https://leetcode.com/problems/find-trending-hashtags-ii)

[中文文档](/solution/3100-3199/3103.Find%20Trending%20Hashtags%20II/README.md)

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
tweet_id is the primary key (column with unique values) for this table.
Each row of this table contains user_id, tweet_id, tweet_date and tweet.
It is guaranteed that all tweet_date are valid dates in February 2024.

</pre>

<p>Hãy viết lời giải để tìm <strong>top</strong> <code>3</code> <strong>hashtag</strong> thịnh hành nhất trong <strong>tháng 2</strong> năm <code>2024</code>. Mỗi tweet có thể chứa <strong>nhiều</strong> <strong>hashtag</strong>.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo số lần xuất hiện của hashtag, sau đó theo hashtag theo </em><strong>thứ tự giảm dần</strong><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Tweets:</p>

<pre class="example-io">
+---------+----------+------------------------------------------------------------+------------+
| user_id | tweet_id | tweet                                                      | tweet_date |
+---------+----------+------------------------------------------------------------+------------+
| 135     | 13       | Enjoying a great start to the day. #HappyDay #MorningVibes | 2024-02-01 |
| 136     | 14       | Another #HappyDay with good vibes! #FeelGood               | 2024-02-03 |
| 137     | 15       | Productivity peaks! #WorkLife #ProductiveDay               | 2024-02-04 |
| 138     | 16       | Exploring new tech frontiers. #TechLife #Innovation        | 2024-02-04 |
| 139     | 17       | Gratitude for today&#39;s moments. #HappyDay #Thankful         | 2024-02-05 |
| 140     | 18       | Innovation drives us. #TechLife #FutureTech                | 2024-02-07 |
| 141     | 19       | Connecting with nature&#39;s serenity. #Nature #Peaceful       | 2024-02-09 |
+---------+----------+------------------------------------------------------------+------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-----------+-------+
| hashtag   | count |
+-----------+-------+
| #HappyDay | 3     |
| #TechLife | 2     |
| #WorkLife | 1     |
+-----------+-------+

</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>#HappyDay:</strong> Xuất hiện trong các tweet có ID 13, 14 và 17, với tổng cộng 3 lần được đề cập.</li>
	<li><strong>#TechLife:</strong> Xuất hiện trong các tweet có ID 16 và 18, với tổng cộng 2 lần được đề cập.</li>
	<li><strong>#WorkLife:</strong> Xuất hiện trong tweet có ID 15, với tổng cộng 1 lần được đề cập.</li>
</ul>

<p><b>Lưu ý:</b> Bảng kết quả được sắp xếp theo thứ tự giảm dần của số lần xuất hiện và hashtag.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So khớp biểu thức chính quy

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán yêu cầu trích xuất mọi hashtag từ các tweet trong tháng 2, sau đó trả về ba hashtag có tần suất và tên theo thứ tự phù hợp. Tự quét từng ký tự có thể làm được nhưng dễ xử lý sai ranh giới hashtag và các lần lặp.
>
> Hashtag bắt đầu bằng `#` và gồm các ký tự từ, vì vậy một regular expression có thể trích xuất chúng trong một lượt. Trước hết, lọc các dòng có ngày trong tháng 2 năm $2024$.
>
> Lọc các tweet trong tháng 2, dùng `findall` để trích xuất hashtag, làm phẳng và đếm chúng, sau đó sắp xếp theo số lần xuất hiện và tên hashtag theo thứ tự giảm dần rồi giữ lại ba dòng đầu tiên.

<!-- thinking:end -->

Ta có thể dùng regular expression để khớp tất cả tag trong mỗi tweet, sau đó đếm số lần xuất hiện của từng tag. Cuối cùng, sắp xếp các tag theo số lần xuất hiện giảm dần. Nếu số lần xuất hiện bằng nhau, sắp xếp theo tên tag giảm dần và trả về ba tag đứng đầu.

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def find_trending_hashtags(tweets: pd.DataFrame) -> pd.DataFrame:
    # Filter tweets for February 2024
    tweets_feb_2024 = tweets[tweets["tweet_date"].between("2024-02-01", "2024-02-29")]

    # Extract hashtags from tweets
    hashtags = tweets_feb_2024["tweet"].str.findall(r"#\w+")

    # Flatten list of hashtags
    all_hashtags = [tag for sublist in hashtags for tag in sublist]

    # Count occurrences of each hashtag
    hashtag_counts = pd.Series(all_hashtags).value_counts().reset_index()
    hashtag_counts.columns = ["hashtag", "count"]

    # Sort by count of hashtag in descending order
    hashtag_counts = hashtag_counts.sort_values(
        by=["count", "hashtag"], ascending=[False, False]
    )

    # Get top 3 trending hashtags
    top_3_hashtags = hashtag_counts.head(3)

    return top_3_hashtags
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
