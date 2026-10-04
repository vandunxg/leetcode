---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [3570. Find Books with No Available Copies](https://leetcode.com/problems/find-books-with-no-available-copies)

[中文文档](/solution/3500-3599/3570.Find%20Books%20with%20No%20Available%20Copies/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>library_books</code></p>

<pre>
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| book_id          | int     |
| title            | varchar |
| author           | varchar |
| genre            | varchar |
| publication_year | int     |
| total_copies     | int     |
+------------------+---------+
book_id là mã định danh duy nhất của bảng này.
Mỗi hàng chứa thông tin về một cuốn sách trong thư viện, bao gồm tổng số bản sao mà thư viện sở hữu.
</pre>

<p>Bảng: <code>borrowing_records</code></p>

<pre>
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| record_id     | int     |
| book_id       | int     |
| borrower_name | varchar |
| borrow_date   | date    |
| return_date   | date    |
+---------------+---------+
record_id là mã định danh duy nhất của bảng này.
Mỗi hàng biểu thị một giao dịch mượn sách, trong đó return_date là NULL nếu sách hiện đang được mượn và chưa được trả.
</pre>

<p>Hãy viết lời giải để tìm <strong>tất cả các cuốn sách</strong> đang <strong>được mượn (chưa trả)</strong> và <strong>không còn bản sao khả dụng</strong> trong thư viện.</p>

<ul>
    <li>Một cuốn sách được xem là <strong>đang được mượn</strong> nếu tồn tại một<strong> </strong>bản ghi mượn có <strong>NULL</strong> ở <code>return_date</code></li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo số người đang mượn theo thứ tự <strong>giảm dần</strong>, sau đó theo tên sách theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>bảng library_books:</p>

<pre class="example-io">
+---------+------------------------+------------------+----------+------------------+--------------+
| book_id | title                  | author           | genre    | publication_year | total_copies |
+---------+------------------------+------------------+----------+------------------+--------------+
| 1       | The Great Gatsby       | F. Scott         | Fiction  | 1925             | 3            |
| 2       | To Kill a Mockingbird  | Harper Lee       | Fiction  | 1960             | 3            |
| 3       | 1984                   | George Orwell    | Dystopian| 1949             | 1            |
| 4       | Pride and Prejudice    | Jane Austen      | Romance  | 1813             | 2            |
| 5       | The Catcher in the Rye | J.D. Salinger    | Fiction  | 1951             | 1            |
| 6       | Brave New World        | Aldous Huxley    | Dystopian| 1932             | 4            |
+---------+------------------------+------------------+----------+------------------+--------------+
</pre>

<p>bảng borrowing_records:</p>

<pre class="example-io">
+-----------+---------+---------------+-------------+-------------+
| record_id | book_id | borrower_name | borrow_date | return_date |
+-----------+---------+---------------+-------------+-------------+
| 1         | 1       | Alice Smith   | 2024-01-15  | NULL        |
| 2         | 1       | Bob Johnson   | 2024-01-20  | NULL        |
| 3         | 2       | Carol White   | 2024-01-10  | 2024-01-25  |
| 4         | 3       | David Brown   | 2024-02-01  | NULL        |
| 5         | 4       | Emma Wilson   | 2024-01-05  | NULL        |
| 6         | 5       | Frank Davis   | 2024-01-18  | 2024-02-10  |
| 7         | 1       | Grace Miller  | 2024-02-05  | NULL        |
| 8         | 6       | Henry Taylor  | 2024-01-12  | NULL        |
| 9         | 2       | Ivan Clark    | 2024-02-12  | NULL        |
| 10        | 2       | Jane Adams    | 2024-02-15  | NULL        |
+-----------+---------+---------------+-------------+-------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+---------+------------------+---------------+-----------+------------------+-------------------+
| book_id | title            | author        | genre     | publication_year | current_borrowers |
+---------+------------------+---------------+-----------+------------------+-------------------+
| 1       | The Great Gatsby | F. Scott      | Fiction   | 1925             | 3                 |
| 3       | 1984             | George Orwell | Dystopian | 1949             | 1                 |
+---------+------------------+---------------+-----------+------------------+-------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>The Great Gatsby (book_id = 1):</strong>

    <ul>
        <li>Tổng số bản sao: 3</li>
        <li>Hiện đang được Alice Smith, Bob Johnson và Grace Miller mượn (3 người mượn)</li>
        <li>Số bản sao còn lại: 3 - 3 = 0</li>
        <li>Được đưa vào kết quả vì available_copies = 0</li>
    </ul>
    </li>
    <li><strong>1984 (book_id = 3):</strong>
    <ul>
        <li>Tổng số bản sao: 1</li>
        <li>Hiện đang được David Brown mượn (1 người mượn)</li>
        <li>Số bản sao còn lại: 1 - 1 = 0</li>
        <li>Được đưa vào kết quả vì available_copies = 0</li>
    </ul>
    </li>
    <li><strong>Các sách không được đưa vào kết quả:</strong>
    <ul>
        <li>To Kill a Mockingbird (book_id = 2): Tổng số bản sao = 3, số người đang mượn = 2, số bản sao còn lại = 1</li>
        <li>Pride and Prejudice (book_id = 4): Tổng số bản sao = 2, số người đang mượn = 1, số bản sao còn lại = 1</li>
        <li>The Catcher in the Rye (book_id = 5): Tổng số bản sao = 1, số người đang mượn = 0, số bản sao còn lại = 1</li>
        <li>Brave New World (book_id = 6): Tổng số bản sao = 4, số người đang mượn = 1, số bản sao còn lại = 3</li>
    </ul>
    </li>
    <li><strong>Thứ tự kết quả:</strong>
    <ul>
        <li>The Great Gatsby đứng đầu với 3 người đang mượn</li>
        <li>1984 đứng thứ hai với 1 người đang mượn</li>
    </ul>
    </li>

</ul>

<p>Bảng kết quả được sắp xếp theo current_borrowers giảm dần, sau đó theo book_title tăng dần.</p>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gom nhóm và tính tổng + Truy vấn nối

<!-- thinking:start -->

> **Tư duy**
>
> Một cuốn sách không còn bản sao khả dụng khi số lượt mượn chưa đóng bằng $\textit{total\_copies}$. Ta đếm các hàng có $\textit{return\_date}$ là null theo $\textit{book\_id}$ rồi inner join với bảng sách.
>
> Giữ lại các sách có $\textit{current\_borrowers} = \textit{total\_copies}$, sắp xếp theo số người mượn giảm dần rồi theo tên sách tăng dần, và chọn các cột được yêu cầu.

<!-- thinking:end -->

Trước tiên, ta đếm số người đang mượn của mỗi cuốn sách, sau đó nối kết quả này với bảng thông tin sách để lọc ra những cuốn có số người đang mượn bằng tổng số bản sao. Cuối cùng, ta sắp xếp kết quả theo số người đang mượn giảm dần; nếu bằng nhau thì sắp xếp theo tên sách tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT book_id, COUNT(1) current_borrowers
        FROM borrowing_records
        WHERE return_date IS NULL
        GROUP BY 1
    )
SELECT book_id, title, author, genre, publication_year, current_borrowers
FROM
    library_books
    JOIN T USING (book_id)
WHERE current_borrowers = total_copies
ORDER BY 6 DESC, 2;
```

#### Pandas

```python
import pandas as pd


def find_books_with_no_available_copies(
    library_books: pd.DataFrame, borrowing_records: pd.DataFrame
) -> pd.DataFrame:
    current_borrowers = (
        borrowing_records[borrowing_records["return_date"].isna()]
        .groupby("book_id")
        .size()
        .rename("current_borrowers")
        .reset_index()
    )

    merged = library_books.merge(current_borrowers, on="book_id", how="inner")
    fully_borrowed = merged[merged["current_borrowers"] == merged["total_copies"]]
    fully_borrowed = fully_borrowed.sort_values(
        by=["current_borrowers", "title"], ascending=[False, True]
    )

    cols = [
        "book_id",
        "title",
        "author",
        "genre",
        "publication_year",
        "current_borrowers",
    ]
    return fully_borrowed[cols].reset_index(drop=True)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
