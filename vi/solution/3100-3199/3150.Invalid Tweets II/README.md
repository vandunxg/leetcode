---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3150. Invalid Tweets II 🔒](https://leetcode.com/problems/invalid-tweets-ii)

[中文文档](/solution/3100-3199/3150.Invalid%20Tweets%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Tweets</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| tweet_id       | int     |
| content        | varchar |
+----------------+---------+
tweet_id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Bảng này chứa tất cả tweet trong một ứng dụng mạng xã hội.
</pre>

<p>Hãy viết lời giải để tìm các <strong>tweet không hợp lệ</strong>. Một tweet được xem là không hợp lệ nếu thỏa mãn <strong>bất kỳ</strong> tiêu chí nào sau đây:</p>

<ul>
	<li>Có độ dài vượt quá <code>140</code> ký tự.</li>
	<li>Có nhiều hơn <code>3</code> mention.</li>
	<li>Có nhiều hơn <code><font face="monospace">3</font></code>&nbsp;hashtag.</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo</em> <code>tweet_id</code> <em>theo <strong>thứ tự tăng dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Tweets:</p>

<pre class="example-io">
  +----------+-----------------------------------------------------------------------------------+
  | tweet_id | content                                                                           |
  +----------+-----------------------------------------------------------------------------------+
  | 1        | Traveling, exploring, and living my best life @JaneSmith @SaraJohnson @LisaTaylor |
  |          | @MikeBrown #Foodie #Fitness #Learning                                             |
  | 2        | Just had the best dinner with friends! #Foodie #Friends #Fun                      |
  | 4        | Working hard on my new project #Work #Goals #Productivity #Fun                    |
  +----------+-----------------------------------------------------------------------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
  +----------+
  | tweet_id |
  +----------+
  | 1        |
  | 4        |
  +----------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>tweet_id&nbsp;1 chứa 4&nbsp;mention.</li>
	<li>tweet_id 4 chứa 4 hashtag.</li>
</ul>
Bảng kết quả được sắp xếp theo tweet_id theo thứ tự tăng dần.</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm LENGTH() + Hàm REPLACE()

<!-- thinking:start -->

> **Tư duy**
>
> Một tweet không hợp lệ nếu dài hơn $140$ ký tự hoặc chứa nhiều hơn ba ký hiệu `@` hoặc `#`. Ba điều kiện này độc lập với nhau.
>
> SQL có thể đếm ký hiệu bằng hiệu giữa `LENGTH` và `LENGTH(REPLACE(...))`; pandas đếm chúng trên cột chuỗi.
>
> Lọc theo phép tuyển của ba điều kiện, giữ lại $tweet\_id$, rồi sắp xếp theo thứ tự tăng dần.

<!-- thinking:end -->

Ta có thể dùng hàm `LENGTH()` để tính độ dài chuỗi, tính độ dài sau khi loại bỏ `@` hoặc `#`, rồi dùng toán tử `OR` để kết nối ba điều kiện này, lọc ra tweet_id tương ứng và sắp xếp tweet_id theo thứ tự tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT tweet_id
FROM Tweets
WHERE
    LENGTH(content) > 140
    OR (LENGTH(content) - LENGTH(REPLACE(content, '@', ''))) > 3
    OR (LENGTH(content) - LENGTH(REPLACE(content, '#', ''))) > 3
ORDER BY 1;
```

#### Python3

```python
import pandas as pd


def find_invalid_tweets(tweets: pd.DataFrame) -> pd.DataFrame:
    invalid_tweets = tweets[
        (tweets["content"].str.len() > 140)
        | (tweets["content"].str.count("@") > 3)
        | (tweets["content"].str.count("#") > 3)
    ].sort_values(by="tweet_id")
    return invalid_tweets[["tweet_id"]]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
