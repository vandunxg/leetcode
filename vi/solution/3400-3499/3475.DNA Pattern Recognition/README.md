---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3475. DNA Pattern Recognition](https://leetcode.com/problems/dna-pattern-recognition)

[中文文档](/solution/3400-3499/3475.DNA%20Pattern%20Recognition/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Samples</code></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| sample_id      | int     |
| dna_sequence   | varchar |
| species        | varchar |
+----------------+---------+
sample_id là khóa duy nhất của bảng này.
Mỗi hàng chứa một chuỗi DNA được biểu diễn bằng các ký tự (A, T, G, C) và loài mà mẫu được thu thập từ đó.
</pre>

<p>Các nhà sinh học đang nghiên cứu những pattern cơ bản trong chuỗi DNA. Hãy viết lời giải để xác định <code>sample_id</code> có các pattern sau:</p>

<ul>
	<li>Các chuỗi <strong>bắt đầu</strong> bằng <strong>ATG</strong>&nbsp;(một <strong>codon bắt đầu</strong> phổ biến)</li>
	<li>Các chuỗi <strong>kết thúc</strong> bằng một trong các chuỗi <strong>TAA</strong>, <strong>TAG</strong> hoặc <strong>TGA</strong>&nbsp;(<strong>codon kết thúc</strong>)</li>
	<li>Các chuỗi chứa motif <strong>ATAT</strong>&nbsp;(một pattern lặp đơn giản)</li>
	<li>Các chuỗi có <strong>ít nhất</strong> <code>3</code> ký tự <strong>G</strong> <strong>liên tiếp</strong>&nbsp;(như <strong>GGG</strong> hoặc <strong>GGGG</strong>)</li>
</ul>

<p><em>Trả về bảng kết quả được sắp xếp theo&nbsp;</em><em><strong>sample_id</strong> tăng dần</em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<p>Bảng Samples:</p>

<pre class="example-io">
+-----------+------------------+-----------+
| sample_id | dna_sequence     | species   |
+-----------+------------------+-----------+
| 1         | ATGCTAGCTAGCTAA  | Human     |
| 2         | GGGTCAATCATC     | Human     |
| 3         | ATATATCGTAGCTA   | Human     |
| 4         | ATGGGGTCATCATAA  | Mouse     |
| 5         | TCAGTCAGTCAG     | Mouse     |
| 6         | ATATCGCGCTAG     | Zebrafish |
| 7         | CGTATGCGTCGTA    | Zebrafish |
+-----------+------------------+-----------+
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
+-----------+------------------+-------------+-------------+------------+------------+------------+
| sample_id | dna_sequence     | species     | has_start   | has_stop   | has_atat   | has_ggg    |
+-----------+------------------+-------------+-------------+------------+------------+------------+
| 1         | ATGCTAGCTAGCTAA  | Human       | 1           | 1          | 0          | 0          |
| 2         | GGGTCAATCATC     | Human       | 0           | 0          | 0          | 1          |
| 3         | ATATATCGTAGCTA   | Human       | 0           | 0          | 1          | 0          |
| 4         | ATGGGGTCATCATAA  | Mouse       | 1           | 1          | 0          | 1          |
| 5         | TCAGTCAGTCAG     | Mouse       | 0           | 0          | 0          | 0          |
| 6         | ATATCGCGCTAG     | Zebrafish   | 0           | 1          | 1          | 0          |
| 7         | CGTATGCGTCGTA    | Zebrafish   | 0           | 0          | 0          | 0          |
+-----------+------------------+-------------+-------------+------------+------------+------------+
</pre>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mẫu 1 (ATGCTAGCTAGCTAA):
	<ul>
		<li>Bắt đầu bằng ATG&nbsp;(has_start = 1)</li>
		<li>Kết thúc bằng TAA&nbsp;(has_stop = 1)</li>
		<li>Không chứa ATAT&nbsp;(has_atat = 0)</li>
		<li>Không chứa ít nhất 3 ký tự &#39;G&#39; liên tiếp (has_ggg = 0)</li>
	</ul>
	</li>
	<li>Mẫu 2 (GGGTCAATCATC):
	<ul>
		<li>Không bắt đầu bằng ATG&nbsp;(has_start = 0)</li>
		<li>Không kết thúc bằng TAA, TAG hoặc TGA&nbsp;(has_stop = 0)</li>
		<li>Không chứa ATAT&nbsp;(has_atat = 0)</li>
		<li>Chứa GGG&nbsp;(has_ggg = 1)</li>
	</ul>
	</li>
	<li>Mẫu 3 (ATATATCGTAGCTA):
	<ul>
		<li>Không bắt đầu bằng ATG&nbsp;(has_start = 0)</li>
		<li>Không kết thúc bằng TAA, TAG hoặc TGA&nbsp;(has_stop = 0)</li>
		<li>Chứa ATAT&nbsp;(has_atat = 1)</li>
		<li>Không chứa ít nhất 3 ký tự &#39;G&#39; liên tiếp (has_ggg = 0)</li>
	</ul>
	</li>
	<li>Mẫu 4 (ATGGGGTCATCATAA):
	<ul>
		<li>Bắt đầu bằng ATG&nbsp;(has_start = 1)</li>
		<li>Kết thúc bằng TAA&nbsp;(has_stop = 1)</li>
		<li>Không chứa ATAT&nbsp;(has_atat = 0)</li>
		<li>Chứa GGGG&nbsp;(has_ggg = 1)</li>
	</ul>
	</li>
	<li>Mẫu 5 (TCAGTCAGTCAG):
	<ul>
		<li>Không khớp với pattern nào (tất cả các trường = 0)</li>
	</ul>
	</li>
	<li>Mẫu 6 (ATATCGCGCTAG):
	<ul>
		<li>Không bắt đầu bằng ATG&nbsp;(has_start = 0)</li>
		<li>Kết thúc bằng TAG&nbsp;(has_stop = 1)</li>
		<li>Bắt đầu bằng ATAT&nbsp;(has_atat = 1)</li>
		<li>Không chứa ít nhất 3 ký tự &#39;G&#39; liên tiếp (has_ggg = 0)</li>
	</ul>
	</li>
	<li>Mẫu 7 (CGTATGCGTCGTA):
	<ul>
		<li>Không bắt đầu bằng ATG&nbsp;(has_start = 0)</li>
		<li>Không kết thúc bằng TAA, &quot;TAG&quot; hoặc &quot;TGA&quot; (has_stop = 0)</li>
		<li>Không chứa ATAT&nbsp;(has_atat = 0)</li>
		<li>Không chứa ít nhất 3 ký tự &#39;G&#39; liên tiếp (has_ggg = 0)</li>
	</ul>
	</li>
</ul>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Kết quả được sắp xếp theo sample_id tăng dần</li>
	<li>Với mỗi pattern, 1 cho biết pattern xuất hiện và 0 cho biết pattern không xuất hiện</li>
</ul>
</div>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Fuzzy Matching + Regular Expressions

<!-- thinking:start -->

> **Tư duy**
>
> Bốn cờ dùng để phát hiện codon bắt đầu, codon kết thúc, $\textit{ATAT}$ và ít nhất ba $G$s liên tiếp. Các phương thức xử lý chuỗi ngắn gọn và an toàn hơn so với tự quét thủ công.
>
> $\textit{startswith}/\textit{endswith}$ kiểm tra hai đầu chuỗi; phép chứa và `GGG+` kiểm tra các pattern bên trong.
>
> Bốn cột có giá trị $0/1$, sau đó frame được sắp xếp theo $\textit{sample\_id}$, tương ứng với lời giải SQL dùng `LIKE`/`REGEXP`.

<!-- thinking:end -->

Ta có thể dùng `LIKE` và `REGEXP` để tìm pattern, trong đó:

- LIKE `'ATG%'` kiểm tra chuỗi có bắt đầu bằng ATG hay không
- REGEXP `'TAA$|TAG$|TGA$'` kiểm tra chuỗi có kết thúc bằng TAA, TAG hoặc TGA hay không ($ biểu thị cuối chuỗi)
- LIKE `'%ATAT%'` kiểm tra chuỗi có chứa ATAT hay không
- REGEXP `'GGG+'` kiểm tra chuỗi có chứa ít nhất 3 ký tự G liên tiếp hay không

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    sample_id,
    dna_sequence,
    species,
    dna_sequence LIKE 'ATG%' AS has_start,
    dna_sequence REGEXP 'TAA$|TAG$|TGA$' AS has_stop,
    dna_sequence LIKE '%ATAT%' AS has_atat,
    dna_sequence REGEXP 'GGG+' AS has_ggg
FROM Samples
ORDER BY 1;
```

#### Pandas

```python
import pandas as pd


def analyze_dna_patterns(samples: pd.DataFrame) -> pd.DataFrame:
    samples["has_start"] = samples["dna_sequence"].str.startswith("ATG").astype(int)
    samples["has_stop"] = (
        samples["dna_sequence"].str.endswith(("TAA", "TAG", "TGA")).astype(int)
    )
    samples["has_atat"] = samples["dna_sequence"].str.contains("ATAT").astype(int)
    samples["has_ggg"] = samples["dna_sequence"].str.contains("GGG+").astype(int)
    return samples.sort_values(by="sample_id").reset_index(drop=True)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
