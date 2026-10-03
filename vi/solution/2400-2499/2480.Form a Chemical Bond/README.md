---
comments: true
difficulty: Easy
tags:
    - Database
---

<!-- problem:start -->

# [2480. Form a Chemical Bond 🔒](https://leetcode.com/problems/form-a-chemical-bond)

[中文文档](/solution/2400-2499/2480.Form%20a%20Chemical%20Bond/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Elements</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| symbol      | varchar |
| type        | enum    |
| electrons   | int     |
+-------------+---------+
symbol là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng của bảng này chứa thông tin về một nguyên tố.
type là một ENUM (phân loại) gồm các loại (&#39;Metal&#39;, &#39;Nonmetal&#39;, &#39;Noble&#39;)
  - Nếu type là Noble, electrons bằng 0.
  - Nếu type là Metal, electrons là số electron mà một nguyên tử của nguyên tố này có thể cho đi.
  - Nếu type là Nonmetal, electrons là số electron mà một nguyên tử của nguyên tố này cần.
</pre>

<p>&nbsp;</p>

<p>Hai nguyên tố có thể tạo liên kết nếu một nguyên tố là <code>&#39;Metal&#39;</code> và nguyên tố còn lại là <code>&#39;Nonmetal&#39;</code>.</p>

<p>Hãy viết lời giải để tìm tất cả các cặp nguyên tố có thể tạo liên kết.</p>

<p>Trả về bảng kết quả <strong>theo bất kỳ thứ tự nào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Elements:
+--------+----------+-----------+
| symbol | type     | electrons |
+--------+----------+-----------+
| He     | Noble    | 0         |
| Na     | Metal    | 1         |
| Ca     | Metal    | 2         |
| La     | Metal    | 3         |
| Cl     | Nonmetal | 1         |
| O      | Nonmetal | 2         |
| N      | Nonmetal | 3         |
+--------+----------+-----------+
<strong>Đầu ra:</strong>
+-------+----------+
| metal | nonmetal |
+-------+----------+
| La    | Cl       |
| Ca    | Cl       |
| Na    | Cl       |
| La    | O        |
| Ca    | O        |
| Na    | O        |
| La    | N        |
| Ca    | N        |
| Na    | N        |
+-------+----------+
<strong>Giải thích:</strong>
Các nguyên tố kim loại là La, Ca và Na.
Các nguyên tố phi kim là Cl, O và N.
Mỗi nguyên tố kim loại ghép cặp với một nguyên tố phi kim trong bảng kết quả.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một liên kết là bất kỳ cặp kim loại–phi kim nào. Dùng self-join trên $\textit{Elements}$ với các loại Metal và Nonmetal, sau đó chọn hai symbol.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT a.symbol AS metal, b.symbol AS nonmetal
FROM
    Elements AS a,
    Elements AS b
WHERE a.type = 'Metal' AND b.type = 'Nonmetal';
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
