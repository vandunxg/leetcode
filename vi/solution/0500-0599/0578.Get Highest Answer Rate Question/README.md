---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [578. Get Highest Answer Rate Question 🔒](https://leetcode.com/problems/get-highest-answer-rate-question)

[中文文档](/solution/0500-0599/0578.Get%20Highest%20Answer%20Rate%20Question/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>SurveyLog</code></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| action      | ENUM |
| question_id | int  |
| answer_id   | int  |
| q_num       | int  |
| timestamp   | int  |
+-------------+------+
Bảng này có thể chứa các hàng trùng lặp.
action là ENUM (danh mục) thuộc một trong các giá trị: &quot;show&quot;, &quot;answer&quot; hoặc &quot;skip&quot;.
Mỗi hàng trong bảng cho biết người dùng có ID = id đã thực hiện hành động với câu hỏi question_id tại thời điểm timestamp.
Nếu hành động của người dùng là &quot;answer&quot;, answer_id sẽ chứa id của câu trả lời đó; nếu không, giá trị sẽ là null.
q_num là số thứ tự của câu hỏi trong phiên hiện tại.
</pre>

<p>&nbsp;</p>

<p><strong>Tỷ lệ trả lời</strong> của một câu hỏi là số lần người dùng trả lời câu hỏi chia cho số lần câu hỏi được hiển thị.</p>

<p>Hãy viết lời giải để tìm câu hỏi có <strong>tỷ lệ trả lời</strong> cao nhất. Nếu nhiều câu hỏi cùng có <strong>tỷ lệ trả lời</strong> tối đa, hãy trả về câu hỏi có <code>question_id</code> nhỏ nhất.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> 
Bảng SurveyLog:
+----+--------+-------------+-----------+-------+-----------+
| id | action | question_id | answer_id | q_num | timestamp |
+----+--------+-------------+-----------+-------+-----------+
| 5  | show   | 285         | null      | 1     | 123       |
| 5  | answer | 285         | 124124    | 1     | 124       |
| 5  | show   | 369         | null      | 2     | 125       |
| 5  | skip   | 369         | null      | 2     | 126       |
+----+--------+-------------+-----------+-------+-----------+
<strong>Đầu ra:</strong> 
+------------+
| survey_log |
+------------+
| 285        |
+------------+
<strong>Giải thích:</strong> 
Câu hỏi 285 được hiển thị 1 lần và được trả lời 1 lần. Tỷ lệ trả lời của câu hỏi 285 là 1.0
Câu hỏi 369 được hiển thị 1 lần nhưng không được trả lời. Tỷ lệ trả lời của câu hỏi 369 là 0.0
Câu hỏi 285 có tỷ lệ trả lời cao nhất.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tỷ lệ trả lời bằng số câu trả lời chia cho số lần hiển thị; chọn tỷ lệ cao nhất và nếu hòa thì lấy `question_id` nhỏ nhất. Chỉ cần một phép group-by.
>
> `SUM(action = 'answer') / SUM(action = 'show')` tính tỷ lệ. Sắp xếp tỷ lệ giảm dần rồi id tăng dần, sau đó dùng `LIMIT 1`.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT question_id AS survey_log
FROM SurveyLog
GROUP BY 1
ORDER BY SUM(action = 'answer') / SUM(action = 'show') DESC, 1
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 gom nhóm bằng `GROUP BY`. Window function có thể tính cùng tỷ lệ cho từng câu hỏi rồi sắp xếp kết quả.
>
> `SUM(...) OVER (PARTITION BY question_id)` gắn tỷ lệ vào từng hàng; query bên ngoài sắp xếp và giữ lại một id. Kết quả chọn ra giống cách group-by và dễ kết hợp với các cột window khác hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
WITH
    T AS (
        SELECT
            question_id AS survey_log,
            (SUM(action = 'answer') OVER (PARTITION BY question_id)) / (
                SUM(action = 'show') OVER (PARTITION BY question_id)
            ) AS ratio
        FROM SurveyLog
    )
SELECT survey_log
FROM T
ORDER BY ratio DESC, 1
LIMIT 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
