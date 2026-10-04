---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3642. Find Books with Polarized Opinions](https://leetcode.com/problems/find-books-with-polarized-opinions)

[Tài liệu tiếng Trung](/solution/3600-3699/3642.Find%20Books%20with%20Polarized%20Opinions/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>books</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| book_id     | int     |
| title       | varchar |
| author      | varchar |
| genre       | varchar |
| pages       | int     |
+-------------+---------+
book_id là ID duy nhất của bảng này.
Mỗi hàng chứa thông tin về một cuốn sách, bao gồm thể loại và số trang.
</pre>

<p>Bảng: <code>reading_sessions</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| session_id     | int     |
| book_id        | int     |
| reader_name    | varchar |
| pages_read     | int     |
| session_rating | int     |
+----------------+---------+
session_id là ID duy nhất của bảng này.
Mỗi hàng biểu diễn một phiên đọc, trong đó một người đọc một phần của cuốn sách. session_rating nằm trong thang điểm từ 1 đến 5.
</pre>

<p>Hãy viết một lời giải để tìm những cuốn sách có <strong>ý kiến phân cực</strong> - những cuốn nhận được cả đánh giá rất cao và đánh giá rất thấp từ những độc giả khác nhau.</p>

<ul>
    <li>Một cuốn sách có ý kiến phân cực nếu có <code>at least one rating &ge; 4</code> và <code>at least one rating &le; 2</code></li>
    <li>Chỉ xét những cuốn sách có <strong>ít nhất </strong><code>5</code><strong> phiên đọc</strong></li>
    <li>Tính <strong>độ chênh lệch đánh giá</strong> theo công thức (<code>highest_rating - lowest_rating</code>)</li>
    <li>Tính <strong>điểm phân cực</strong> bằng số đánh giá cực đoan (<code>ratings &le; 2 or &ge; 4</code>) chia cho tổng số phiên đọc</li>
    <li><strong>Chỉ bao gồm</strong> những cuốn sách có <code>polarization score &ge; 0.6</code> (ít nhất <code>60%</code> đánh giá cực đoan)</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo điểm phân cực theo thứ tự <strong>giảm dần</strong>, sau đó theo tiêu đề theo thứ tự <strong>giảm dần</strong></em>.<br />
<em>Điểm phân cực</em> phải được làm tròn đến 2 chữ số thập phân.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng books:</p>

<pre class="example-io">
+---------+------------------------+---------------+----------+-------+
| book_id | title                  | author        | genre    | pages |
+---------+------------------------+---------------+----------+-------+
| 1       | The Great Gatsby       | F. Scott      | Fiction  | 180   |
| 2       | To Kill a Mockingbird  | Harper Lee    | Fiction  | 281   |
| 3       | 1984                   | George Orwell | Dystopian| 328   |
| 4       | Pride and Prejudice    | Jane Austen   | Romance  | 432   |
| 5       | The Catcher in the Rye | J.D. Salinger | Fiction  | 277   |
+---------+------------------------+---------------+----------+-------+
</pre>

<p>bảng reading_sessions:</p>

<pre class="example-io">
+------------+---------+-------------+------------+----------------+
| session_id | book_id | reader_name | pages_read | session_rating |
+------------+---------+-------------+------------+----------------+
| 1          | 1       | Alice       | 50         | 5              |
| 2          | 1       | Bob         | 60         | 1              |
| 3          | 1       | Carol       | 40         | 4              |
| 4          | 1       | David       | 30         | 2              |
| 5          | 1       | Emma        | 45         | 5              |
| 6          | 2       | Frank       | 80         | 4              |
| 7          | 2       | Grace       | 70         | 4              |
| 8          | 2       | Henry       | 90         | 5              |
| 9          | 2       | Ivy         | 60         | 4              |
| 10         | 2       | Jack        | 75         | 4              |
| 11         | 3       | Kate        | 100        | 2              |
| 12         | 3       | Liam        | 120        | 1              |
| 13         | 3       | Mia         | 80         | 2              |
| 14         | 3       | Noah        | 90         | 1              |
| 15         | 3       | Olivia      | 110        | 4              |
| 16         | 3       | Paul        | 95         | 5              |
| 17         | 4       | Quinn       | 150        | 3              |
| 18         | 4       | Ruby        | 140        | 3              |
| 19         | 5       | Sam         | 80         | 1              |
| 20         | 5       | Tara        | 70         | 2              |
+------------+---------+-------------+------------+----------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+------------------+---------------+-----------+-------+---------------+--------------------+
| book_id | title            | author        | genre     | pages | rating_spread | polarization_score |
+---------+------------------+---------------+-----------+-------+---------------+--------------------+
| 1       | The Great Gatsby | F. Scott      | Fiction   | 180   | 4             | 1.00               |
| 3       | 1984             | George Orwell | Dystopian | 328   | 4             | 1.00               |
+---------+------------------+---------------+-----------+-------+---------------+--------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>The Great Gatsby (book_id = 1):</strong>

    <ul>
        <li>Có 5 phiên đọc (đáp ứng yêu cầu tối thiểu)</li>
        <li>Các đánh giá: 5, 1, 4, 2, 5</li>
        <li>Có các đánh giá &ge; 4: 5, 4, 5 (3 phiên)</li>
        <li>Có các đánh giá &le; 2: 1, 2 (2 phiên)</li>
        <li>Độ chênh lệch đánh giá: 5 - 1 = 4</li>
        <li>Đánh giá cực đoan (&le;2 hoặc &ge;4): Cả 5 phiên (5, 1, 4, 2, 5)</li>
        <li>Điểm phân cực: 5/5 = 1.00 (&ge; 0.6, đạt yêu cầu)</li>
    </ul>
    </li>
    <li><strong>1984 (book_id = 3):</strong>
    <ul>
        <li>Có 6 phiên đọc (đáp ứng yêu cầu tối thiểu)</li>
        <li>Các đánh giá: 2, 1, 2, 1, 4, 5</li>
        <li>Có các đánh giá &ge; 4: 4, 5 (2 phiên)</li>
        <li>Có các đánh giá &le; 2: 2, 1, 2, 1 (4 phiên)</li>
        <li>Độ chênh lệch đánh giá: 5 - 1 = 4</li>
        <li>Đánh giá cực đoan (&le;2 hoặc &ge;4): Cả 6 phiên (2, 1, 2, 1, 4, 5)</li>
        <li>Điểm phân cực: 6/6 = 1.00 (&ge; 0.6, đạt yêu cầu)</li>
    </ul>
    </li>
    <li><strong>Các cuốn sách không được đưa vào:</strong>
    <ul>
        <li>To Kill a Mockingbird (book_id = 2): Tất cả đánh giá đều là 4-5, không có đánh giá thấp (&le;2)</li>
        <li>Pride and Prejudice (book_id = 4): Chỉ có 2 phiên (&lt; mức tối thiểu là 5)</li>
        <li>The Catcher in the Rye (book_id = 5): Chỉ có 2 phiên (&lt; mức tối thiểu là 5)</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo điểm phân cực giảm dần, sau đó theo tiêu đề sách giảm dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Join + Gom nhóm và tổng hợp

<!-- thinking:start -->

> **Tư duy**
>
> Sự phân cực phụ thuộc vào các giá trị cực trị của đánh giá, số phiên đọc và tỷ lệ điểm cực đoan, vì vậy cần tổng hợp theo sách trước khi lọc. Join $\textit{books}$ với các phiên đọc rồi group theo sách.
>
> Một phép tổng hợp có thể lấy giá trị lớn nhất, nhỏ nhất, độ chênh lệch, số phiên đọc và số đánh giá $\le 2$ hoặc $\ge 4$, sau đó làm tròn theo kiểu half-up cho tỷ lệ phân cực.
>
> Giữ lại những cuốn sách có ít nhất năm phiên đọc, đồng thời có cả đánh giá cao và thấp, và tỷ lệ ít nhất là $0.6$; sắp xếp theo tỷ lệ rồi theo tiêu đề, đều theo thứ tự giảm dần.

<!-- thinking:end -->

Ta có thể thực hiện việc này bằng cách join bảng `books` với bảng `reading_sessions`, sau đó group và aggregate kết quả.

Trước tiên, cần tính khoảng đánh giá, số đánh giá cực đoan và tỷ lệ đánh giá cực đoan cho mỗi cuốn sách.

Sau đó, có thể lọc những cuốn sách đáp ứng các tiêu chí dựa trên những chỉ số này.

Cuối cùng, sắp xếp kết quả theo tỷ lệ đánh giá cực đoan và tiêu đề sách theo thứ tự giảm dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    book_id,
    title,
    author,
    genre,
    pages,
    (MAX(session_rating) - MIN(session_rating)) AS rating_spread,
    ROUND((SUM(session_rating <= 2) + SUM(session_rating >= 4)) / COUNT(1), 2) polarization_score
FROM
    books
    JOIN reading_sessions USING (book_id)
GROUP BY book_id
HAVING
    COUNT(1) >= 5
    AND MAX(session_rating) >= 4
    AND MIN(session_rating) <= 2
    AND polarization_score >= 0.6
ORDER BY polarization_score DESC, title DESC;
```

#### Pandas

```python
import pandas as pd
from decimal import Decimal, ROUND_HALF_UP


def find_polarized_books(
    books: pd.DataFrame, reading_sessions: pd.DataFrame
) -> pd.DataFrame:
    df = books.merge(reading_sessions, on="book_id")
    agg_df = (
        df.groupby(["book_id", "title", "author", "genre", "pages"])
        .agg(
            max_rating=("session_rating", "max"),
            min_rating=("session_rating", "min"),
            rating_spread=("session_rating", lambda x: x.max() - x.min()),
            count_sessions=("session_rating", "count"),
            low_or_high_count=("session_rating", lambda x: ((x <= 2) | (x >= 4)).sum()),
        )
        .reset_index()
    )

    agg_df["polarization_score"] = agg_df.apply(
        lambda r: float(
            Decimal(r["low_or_high_count"] / r["count_sessions"]).quantize(
                Decimal("0.01"), rounding=ROUND_HALF_UP
            )
        ),
        axis=1,
    )

    result = agg_df[
        (agg_df["count_sessions"] >= 5)
        & (agg_df["max_rating"] >= 4)
        & (agg_df["min_rating"] <= 2)
        & (agg_df["polarization_score"] >= 0.6)
    ]

    return result.sort_values(
        by=["polarization_score", "title"], ascending=[False, False]
    )[
        [
            "book_id",
            "title",
            "author",
            "genre",
            "pages",
            "rating_spread",
            "polarization_score",
        ]
    ]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
