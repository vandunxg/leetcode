---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3374. First Letter Capitalization II](https://leetcode.com/problems/first-letter-capitalization-ii)

[中文文档](/solution/3300-3399/3374.First%20Letter%20Capitalization%20II/README.md)

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
	<li>Chuyển <strong>chữ cái đầu tiên</strong> của mỗi từ thành <strong>chữ hoa</strong> và chuyển các chữ cái <strong>còn lại</strong> thành <strong>chữ thường</strong></li>
	<li>Xử lý đặc biệt đối với các từ chứa ký tự đặc biệt:
	<ul>
		<li>Đối với các từ được nối bằng dấu gạch ngang <code>-</code>, <strong>cả hai phần</strong> đều phải được <strong>viết hoa chữ cái đầu</strong> (<strong>ví dụ</strong>, top-rated&nbsp;&rarr; Top-Rated)</li>
	</ul>
	</li>
	<li>Tất cả <strong>định dạng</strong> và <strong>khoảng trắng</strong> khác phải được <strong>giữ nguyên</strong></li>
</ul>

<p>Trả về <em>bảng kết quả bao gồm cả <code>content_text</code> ban đầu và văn bản đã được biến đổi theo các quy tắc trên</em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng user_content:</p>

<pre class="example-io">
+------------+---------------------------------+
| content_id | content_text                    |
+------------+---------------------------------+
| 1          | hello world of SQL              |
| 2          | the QUICK-brown fox             |
| 3          | modern-day DATA science         |
| 4          | web-based FRONT-end development |
+------------+---------------------------------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+------------+---------------------------------+---------------------------------+
| content_id | original_text                   | converted_text                  |
+------------+---------------------------------+---------------------------------+
| 1          | hello world of SQL              | Hello World Of Sql              |
| 2          | the QUICK-brown fox             | The Quick-Brown Fox             |
| 3          | modern-day DATA science         | Modern-Day Data Science         |
| 4          | web-based FRONT-end development | Web-Based Front-End Development |
+------------+---------------------------------+---------------------------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với content_id = 1:
	<ul>
		<li>Chữ cái đầu tiên của mỗi từ được viết hoa: &quot;Hello World Of Sql&quot;</li>
	</ul>
	</li>
	<li>Với content_id = 2:
	<ul>
		<li>Chứa từ có dấu gạch ngang &quot;QUICK-brown&quot;, được chuyển thành &quot;Quick-Brown&quot;</li>
		<li>Các từ khác tuân theo quy tắc viết hoa thông thường</li>
	</ul>
	</li>
	<li>Với content_id = 3:
	<ul>
		<li>Từ có dấu gạch ngang &quot;modern-day&quot; được chuyển thành &quot;Modern-Day&quot;</li>
		<li>&quot;DATA&quot; được chuyển thành &quot;Data&quot;</li>
	</ul>
	</li>
	<li>Với content_id = 4:
	<ul>
		<li>Chứa hai từ có dấu gạch ngang: &quot;web-based&quot; &rarr; &quot;Web-Based&quot;</li>
		<li>Và &quot;FRONT-end&quot; &rarr; &quot;Front-End&quot;</li>
	</ul>
	</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>context_text</code> chỉ chứa các chữ cái tiếng Anh và các ký tự trong danh sách <code>[&#39;\&#39;, &#39; &#39;, &#39;@&#39;, &#39;-&#39;, &#39;/&#39;, &#39;^&#39;, &#39;,&#39;]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ngoài việc viết hoa các từ như ở phần I, mỗi phần được ngăn cách bằng dấu gạch ngang cũng phải được viết hoa chữ cái đầu riêng.
>
> Tách theo dấu cách; nếu một token chứa `-`, tiếp tục tách token đó và dùng $\textit{capitalize}$ cho từng phần.
>
> Vì vậy, `foo-bar` trở thành `Foo-Bar`, còn các phần còn lại được xử lý như ở phần I.

<!-- thinking:end -->

<!-- tabs:start -->

#### Pandas

```python
import pandas as pd


def capitalize_content(user_content: pd.DataFrame) -> pd.DataFrame:
    def convert_text(text: str) -> str:
        return " ".join(
            (
                "-".join([part.capitalize() for part in word.split("-")])
                if "-" in word
                else word.capitalize()
            )
            for word in text.split(" ")
        )

    user_content["converted_text"] = user_content["content_text"].apply(convert_text)
    return user_content.rename(columns={"content_text": "original_text"})[
        ["content_id", "original_text", "converted_text"]
    ]
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
