---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3358. Books with NULL Ratings 🔒](https://leetcode.com/problems/books-with-null-ratings)

[中文文档](/solution/3300-3399/3358.Books%20with%20NULL%20Ratings/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>books</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| book_id        | int     |
| title          | varchar |
| author         | varchar |
| published_year | int     |
| rating         | decimal |
+----------------+---------+
book_id là khóa duy nhất của bảng này.
Mỗi hàng của bảng này chứa thông tin về một cuốn sách, bao gồm ID duy nhất, tên sách, tác giả, năm xuất bản và rating.
rating có thể là NULL, cho biết cuốn sách chưa được đánh giá.
</pre>

<p>Hãy viết lời giải để tìm tất cả những cuốn sách chưa được đánh giá (tức là có rating bằng <strong>NULL</strong>).</p>

<p><em>Kết quả</em> cần được <em>sắp xếp theo</em> <code>book_id</code> theo thứ tự <strong>tăng dần</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng books:</p>

<pre class="example-io">
+---------+------------------------+------------------+----------------+--------+
| book_id | title                  | author           | published_year | rating |
+---------+------------------------+------------------+----------------+--------+
| 1       | The Great Gatsby       | F. Scott         | 1925           | 4.5    |
| 2       | To Kill a Mockingbird  | Harper Lee       | 1960           | NULL   |
| 3       | Pride and Prejudice    | Jane Austen      | 1813           | 4.8    |
| 4       | The Catcher in the Rye | J.D. Salinger    | 1951           | NULL   |
| 5       | Animal Farm            | George Orwell    | 1945           | 4.2    |
| 6       | Lord of the Flies      | William Golding  | 1954           | NULL   |
+---------+------------------------+------------------+----------------+--------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+------------------------+------------------+----------------+
| book_id | title                  | author           | published_year |
+---------+------------------------+------------------+----------------+
| 2       | To Kill a Mockingbird  | Harper Lee       | 1960           |
| 4       | The Catcher in the Rye | J.D. Salinger    | 1951           |
| 6       | Lord of the Flies      | William Golding  | 1954           |
+---------+------------------------+------------------+----------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các cuốn sách có book_id 2, 4 và 6 có rating là NULL.</li>
    <li>Những cuốn sách này được đưa vào bảng kết quả.</li>
    <li>Các cuốn sách còn lại (book_id 1, 3 và 5) đã có rating nên không được đưa vào.</li>
</ul>
Bảng kết quả được sắp xếp theo book_id theo thứ tự tăng dần</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lọc có điều kiện

<!-- thinking:start -->

> **Tư duy**
>
> Ta liệt kê các cuốn sách chưa có rating, sắp xếp theo $\textit{book\_id}$ và chỉ giữ lại các cột được yêu cầu.
>
> Dùng $\textit{isnull}$ để lọc theo $\textit{rating}$, chọn bốn cột rồi sắp xếp. Không cần dùng phép join hay phép gom nhóm.

<!-- thinking:end -->

Ta lọc trực tiếp những cuốn sách có `rating` bằng `NULL`, sau đó sắp xếp chúng theo `book_id` theo thứ tự tăng dần.

Lưu ý rằng tập kết quả chỉ được chứa các trường `book_id`, `title`, `author` và `published_year`.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT book_id, title, author, published_year
FROM books
WHERE rating IS NULL
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def find_unrated_books(books: pd.DataFrame) -> pd.DataFrame:
    unrated_books = books[books["rating"].isnull()]
    return unrated_books[["book_id", "title", "author", "published_year"]].sort_values(
        by="book_id"
    )
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
