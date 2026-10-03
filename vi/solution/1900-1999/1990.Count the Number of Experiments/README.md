---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1990. Count the Number of Experiments 🔒](https://leetcode.com/problems/count-the-number-of-experiments)

[中文文档](/solution/1900-1999/1990.Count%20the%20Number%20of%20Experiments/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Experiments</code></p>

<pre>
+-----------------+------+
| Column Name     | Type |
+-----------------+------+
| experiment_id   | int  |
| platform        | enum |
| experiment_name | enum |
+-----------------+------+
experiment_id là cột chứa các giá trị duy nhất của bảng này.
platform là kiểu enum (danh mục) với các giá trị (&#39;Android&#39;, &#39;IOS&#39;, &#39;Web&#39;).
experiment_name là kiểu enum (danh mục) với các giá trị (&#39;Reading&#39;, &#39;Sports&#39;, &#39;Programming&#39;).
Bảng này chứa thông tin về ID của một thí nghiệm được thực hiện với một người ngẫu nhiên, nền tảng được dùng để thực hiện thí nghiệm và tên của thí nghiệm.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để báo cáo <strong>số lượng thí nghiệm</strong> được thực hiện trên mỗi trong ba nền tảng, cho từng trong ba thí nghiệm đã cho. Lưu ý rằng tất cả các cặp (platform, experiment) đều phải được đưa vào kết quả, <strong>bao gồm</strong> cả những cặp có <strong>0 thí nghiệm</strong>.</p>

<p>Trả về bảng kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Experiments:
+---------------+----------+-----------------+
| experiment_id | platform | experiment_name |
+---------------+----------+-----------------+
| 4             | IOS      | Programming     |
| 13            | IOS      | Sports          |
| 14            | Android  | Reading         |
| 8             | Web      | Reading         |
| 12            | Web      | Reading         |
| 18            | Web      | Programming     |
+---------------+----------+-----------------+
<strong>Đầu ra:</strong>
+----------+-----------------+-----------------+
| platform | experiment_name | num_experiments |
+----------+-----------------+-----------------+
| Android  | Reading         | 1               |
| Android  | Sports          | 0               |
| Android  | Programming     | 0               |
| IOS      | Reading         | 0               |
| IOS      | Sports          | 1               |
| IOS      | Programming     | 1               |
| Web      | Reading         | 2               |
| Web      | Sports          | 0               |
| Web      | Programming     | 1               |
+----------+-----------------+-----------------+
<strong>Giải thích:</strong>
Trên nền tảng &quot;Android&quot;, chỉ có một thí nghiệm &quot;Reading&quot;.
Trên nền tảng &quot;IOS&quot;, có một thí nghiệm &quot;Sports&quot; và một thí nghiệm &quot;Programming&quot;.
Trên nền tảng &quot;Web&quot;, có hai thí nghiệm &quot;Reading&quot; và một thí nghiệm &quot;Programming&quot;.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mọi cặp nền tảng-thí nghiệm đều phải xuất hiện, kể cả những cặp có giá trị 0. Tích Descartes của ba nền tảng và ba tên thí nghiệm tạo ra tất cả các cặp, sau đó left join với bảng dữ liệu và đếm số hàng của từng cặp.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    P AS (
        SELECT 'Android' AS platform
        UNION
        SELECT 'IOS'
        UNION
        SELECT 'Web'
    ),
    Exp AS (
        SELECT 'Reading' AS experiment_name
        UNION
        SELECT 'Sports'
        UNION
        SELECT 'Programming'
    ),
    T AS (
        SELECT *
        FROM
            P,
            Exp
    )
SELECT platform, experiment_name, COUNT(experiment_id) AS num_experiments
FROM
    T AS t
    LEFT JOIN Experiments USING (platform, experiment_name)
GROUP BY 1, 2;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
