---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2738. Count Occurrences in Text 🔒](https://leetcode.com/problems/count-occurrences-in-text)

[中文文档](/solution/2700-2799/2738.Count%20Occurrences%20in%20Text/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng:<font face="monospace"> <code>Files</code></font></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-- ----------+---------+
| file_name   | varchar |
| content     | text    |
+-------------+---------+
file_name là cột có các giá trị duy nhất của bảng này.
Mỗi hàng chứa file_name và nội dung của file đó.
</pre>

<p>Viết lời giải để tìm số lượng file có ít nhất một lần xuất hiện của các từ&nbsp;<strong>&#39;bull&#39;</strong> và <strong>&#39;bear&#39;</strong> lần lượt dưới dạng <strong>từ độc lập</strong>, bỏ qua mọi trường hợp từ xuất hiện mà không có dấu cách ở cả hai bên (ví dụ: &#39;bullet&#39;,&nbsp;&#39;bears&#39;, &#39;bull.&#39; hoặc &#39;bear&#39;&nbsp;ở đầu hoặc cuối câu sẽ <strong>không được tính</strong>)&nbsp;</p>

<p>Trả về <em>từ &#39;bull&#39; và &#39;bear&#39; cùng với số lần xuất hiện tương ứng theo <strong>bất kỳ thứ tự nào.</strong></em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>&nbsp;
Bảng Files:
+------------+----------------------------------------------------------------------------------+
| file_name  | content                                                                         |
+------------+----------------------------------------------------------------------------------+
| draft1.txt | The stock exchange predicts a bull market which would make many investors happy. |
| draft2.txt | The stock exchange predicts a bull market which would make many investors happy, |
|&nbsp;           | but analysts warn of possibility of too much optimism and that in fact we are    |
|&nbsp;           | awaiting a bear market.                                                          |
| draft3.txt | The stock exchange predicts a bull market which would make many investors happy, |
|&nbsp;           | but analysts warn of possibility of too much optimism and that in fact we are    |
|&nbsp;           | awaiting a bear market. As always predicting the future market is an uncertain   |
|            | game and all investors should follow their instincts and best practices.         |
+------------+----------------------------------------------------------------------------------+
<strong>Đầu ra:</strong>&nbsp;
+------+-------+
| word | count | &nbsp;
+------+-------+
| bull |&nbsp;3     |&nbsp;
| bear |&nbsp;2     |
+------+-------+
<strong>Giải thích:</strong>&nbsp;
- Từ "bull" xuất hiện 1 lần trong "draft1.txt", 1 lần trong "draft2.txt" và 1 lần trong "draft3.txt". Vì vậy, tổng số lần xuất hiện của từ "bull" là 3.
- Từ "bear" xuất hiện 1 lần trong "draft2.txt" và 1 lần trong "draft3.txt". Vì vậy, tổng số lần xuất hiện của từ "bear" là 2.

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số lần xuất hiện độc lập của $bull$ và $bear$. Có thể tách token bằng regular expression, nhưng chỉ có hai từ cần tìm và chúng phải được phân cách bằng dấu cách.
>
> Đếm mỗi từ bằng $LIKE\ '\%\ word\ \%'$ rồi $UNION$ hai hàng, nhờ đó không tính các chuỗi con nằm bên trong token khác.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT 'bull' AS word, COUNT(*) AS count
FROM Files
WHERE content LIKE '% bull %'
UNION
SELECT 'bear' AS word, COUNT(*) AS count
FROM Files
WHERE content LIKE '% bear %';
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
