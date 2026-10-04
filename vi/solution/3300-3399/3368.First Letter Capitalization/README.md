---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3368. First Letter Capitalization 🔒](https://leetcode.com/problems/first-letter-capitalization)

[中文文档](/solution/3300-3399/3368.First%20Letter%20Capitalization/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>user_content</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| content_id  | int     |
| content_text| varchar |
+-------------+---------+
content_id là khóa duy nhất của bảng này.
Mỗi hàng chứa một ID duy nhất và nội dung văn bản tương ứng.
</pre>

<p>Hãy viết lời giải để biến đổi văn bản trong cột <code>content_text</code> bằng cách áp dụng các quy tắc sau:</p>

<ul>
	<li>Chuyển chữ cái đầu tiên của mỗi từ thành chữ hoa</li>
	<li>Giữ tất cả các chữ cái còn lại ở dạng chữ thường</li>
	<li>Giữ nguyên tất cả khoảng trắng hiện có</li>
</ul>

<p><strong>Lưu ý</strong>: Sẽ không có ký tự đặc biệt nào trong <code>content_text</code>.</p>

<p>Trả về <em>bảng kết quả chứa cả <code>content_text</code> ban đầu và văn bản đã biến đổi, trong đó mỗi từ bắt đầu bằng một chữ cái viết hoa</em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng user_content:</p>

<pre class="example-io">
+------------+-----------------------------------+
| content_id | content_text                      |
+------------+-----------------------------------+
| 1          | hello world of SQL                |
| 2          | the QUICK brown fox               |
| 3          | data science AND machine learning |
| 4          | TOP rated programming BOOKS       |
+------------+-----------------------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------+-----------------------------------+-----------------------------------+
| content_id | original_text                     | converted_text                    |
+------------+-----------------------------------+-----------------------------------+
| 1          | hello world of SQL                | Hello World Of Sql                |
| 2          | the QUICK brown fox               | The Quick Brown Fox               |
| 3          | data science AND machine learning | Data Science And Machine Learning |
| 4          | TOP rated programming BOOKS       | Top Rated Programming Books       |
+------------+-----------------------------------+-----------------------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với content_id = 1:
	<ul>
		<li>Chữ cái đầu tiên của mỗi từ được viết hoa: Hello World Of Sql</li>
	</ul>
	</li>
	<li>Với content_id = 2:
	<ul>
		<li>Văn bản ban đầu có chữ hoa và chữ thường lẫn nhau được chuyển thành dạng viết hoa chữ cái đầu mỗi từ: The Quick Brown Fox</li>
	</ul>
	</li>
	<li>Với content_id = 3:
	<ul>
		<li>Từ AND được chuyển thành "And": "Data Science And Machine Learning"</li>
	</ul>
	</li>
	<li>Với content_id = 4:
	<ul>
		<li>Xử lý đúng từ TOP rated: Top Rated</li>
		<li>Chuyển BOOKS từ toàn chữ hoa thành dạng viết hoa chữ cái đầu mỗi từ: Books</li>
	</ul>
	</li>
</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi từ phải bắt đầu bằng chữ hoa và tiếp tục bằng chữ thường. Tách theo dấu cách, dùng $\textit{capitalize}$ cho từng token rồi nối chúng lại.
>
> Phải giữ nguyên các khoảng trắng liên tiếp, vì vậy ta tách theo $\texttt{' '}$ thay vì theo khoảng trắng bất kỳ.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
WITH RECURSIVE
    capitalized_words AS (
        SELECT
            content_id,
            content_text,
            SUBSTRING_INDEX(content_text, ' ', 1) AS word,
            SUBSTRING(
                content_text,
                LENGTH(SUBSTRING_INDEX(content_text, ' ', 1)) + 2
            ) AS remaining_text,
            CONCAT(
                UPPER(LEFT(SUBSTRING_INDEX(content_text, ' ', 1), 1)),
                LOWER(SUBSTRING(SUBSTRING_INDEX(content_text, ' ', 1), 2))
            ) AS processed_word
        FROM user_content
        UNION ALL
        SELECT
            c.content_id,
            c.content_text,
            SUBSTRING_INDEX(c.remaining_text, ' ', 1),
            SUBSTRING(c.remaining_text, LENGTH(SUBSTRING_INDEX(c.remaining_text, ' ', 1)) + 2),
            CONCAT(
                c.processed_word,
                ' ',
                CONCAT(
                    UPPER(LEFT(SUBSTRING_INDEX(c.remaining_text, ' ', 1), 1)),
                    LOWER(SUBSTRING(SUBSTRING_INDEX(c.remaining_text, ' ', 1), 2))
                )
            )
        FROM capitalized_words c
        WHERE c.remaining_text != ''
    )
SELECT
    content_id,
    content_text AS original_text,
    MAX(processed_word) AS converted_text
FROM capitalized_words
GROUP BY 1, 2;
```

#### Pandas

```python
import pandas as pd


def process_text(user_content: pd.DataFrame) -> pd.DataFrame:
    user_content["converted_text"] = user_content["content_text"].apply(
        lambda text: " ".join(word.capitalize() for word in text.split(" "))
    )
    return user_content[["content_id", "content_text", "converted_text"]].rename(
        columns={"content_text": "original_text"}
    )
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
