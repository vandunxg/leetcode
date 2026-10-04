---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3052. Maximize Items 🔒](https://leetcode.com/problems/maximize-items)

[中文文档](/solution/3000-3099/3052.Maximize%20Items/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace"><code>Inventory</code></font></p>

<pre>
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| item_id        | int     |
| item_type      | varchar |
| item_category  | varchar |
| square_footage | decimal |
+----------------+---------+
item_id là cột chứa các giá trị duy nhất của bảng này.
Mỗi hàng bao gồm mã mặt hàng, loại mặt hàng, danh mục mặt hàng và diện tích tính bằng foot vuông.
</pre>

<p>Kho hàng của LeetCode muốn tối đa hóa số lượng mặt hàng có thể lưu trữ trong một kho rộng <code>500,000</code> foot vuông. Kho muốn lưu trữ nhiều mặt hàng <strong>prime</strong> nhất có thể, sau đó dùng diện tích <strong>còn lại</strong> để lưu trữ nhiều mặt hàng <strong>not_prime</strong> nhất có thể.</p>

<p>Hãy viết lời giải để tìm số lượng mặt hàng <strong>prime</strong> và <strong>not_prime</strong> có thể được <strong>lưu trữ</strong> trong kho rộng <code>500,000</code> foot vuông. Xuất loại mặt hàng với <code>prime_eligible</code> trước, tiếp theo là <code>not_prime</code>, cùng với số lượng mặt hàng tối đa có thể lưu trữ.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Số lượng</strong> mặt hàng phải là một số nguyên.</li>
	<li>Nếu số lượng của danh mục <strong>not_prime</strong> là <code>0</code>, cần <strong>xuất</strong> <code>0</code> cho danh mục đó.</li>
</ul>

<p>Trả về <em>bảng kết quả được sắp xếp theo số lượng mặt hàng theo <strong>thứ tự giảm dần</strong></em>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Inventory:
+---------+----------------+---------------+----------------+
| item_id | item_type      | item_category | square_footage |
+---------+----------------+---------------+----------------+
| 1374    | prime_eligible | Watches       | 68.00          |
| 4245    | not_prime      | Art           | 26.40          |
| 5743    | prime_eligible | Software      | 325.00         |
| 8543    | not_prime      | Clothing      | 64.50          |
| 2556    | not_prime      | Shoes         | 15.00          |
| 2452    | prime_eligible | Scientific    | 85.00          |
| 3255    | not_prime      | Furniture     | 22.60          |
| 1672    | prime_eligible | Beauty        | 8.50           |
| 4256    | prime_eligible | Furniture     | 55.50          |
| 6325    | prime_eligible | Food          | 13.20          |
+---------+----------------+---------------+----------------+
<strong>Đầu ra:</strong>
+----------------+-------------+
| item_type      | item_count  |
+----------------+-------------+
| prime_eligible | 5400        |
| not_prime      | 8           |
+----------------+-------------+
<strong>Giải thích:</strong>
- Danh mục prime-eligible gồm tổng cộng 6 mặt hàng, với tổng diện tích là 555.20 (68 + 325 + 85 + 8.50 + 55.50 + 13.20). Có thể lưu trữ 900 bộ gồm 6 mặt hàng này, tương đương 5400 mặt hàng và chiếm 499,680 foot vuông.
- Danh mục not_prime có tổng cộng 4 mặt hàng với tổng diện tích là 128.50. Sau khi trừ diện tích đã dùng cho các mặt hàng prime-eligible (500,000 - 499,680 = 320), còn đủ chỗ cho 2 bộ mặt hàng not_prime, lưu trữ được 8 mặt hàng not_prime trong 320 foot vuông còn lại.
Bảng kết quả được sắp xếp theo số lượng mặt hàng giảm dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Truy vấn Join + Union All

<!-- thinking:start -->

> **Tư duy**
>
> Kho chứa được $500000$ và trước tiên phải được lấp đầy bằng các bộ prime, sau đó dùng diện tích còn lại cho các bộ not-prime. Một bộ là một bản sao của mỗi mặt hàng thuộc loại đó.
>
> Một bộ prime có diện tích $s$, là tổng diện tích của các mặt hàng prime, nên ta lưu trữ được $\lfloor 500000/s \rfloor$ bộ; phần diện tích còn lại được dùng cho tổng diện tích của các mặt hàng not-prime.
>
> Tính $s$, nhân số lượng mặt hàng của mỗi loại với số bộ tương ứng, rồi trả về hai hàng bằng $\texttt{UNION ALL}$.

<!-- thinking:end -->

Trước tiên, ta tính tổng diện tích của tất cả mặt hàng thuộc loại `prime_eligible` và ghi kết quả vào trường `s` của bảng `T`.

Tiếp theo, ta tính lần lượt số lượng mặt hàng thuộc loại `prime_eligible` và `not_prime`. Với mặt hàng loại `prime_eligible`, số bộ có thể lưu trữ là $\lfloor \frac{500000}{s} \rfloor$. Với mặt hàng loại `not_prime`, số bộ có thể lưu trữ là $\lfloor \frac{500000 \mod s}{\sum \textit{s1}} \rfloor$. Trong đó, $\sum \textit{s1}$ là tổng diện tích của tất cả mặt hàng loại `not_prime`. Nhân với số lượng mặt hàng thuộc hai loại `prime_eligible` và `not_prime` tương ứng, ta thu được kết quả.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT SUM(square_footage) AS s
        FROM Inventory
        WHERE item_type = 'prime_eligible'
    )
SELECT
    'prime_eligible' AS item_type,
    COUNT(1) * FLOOR(500000 / s) AS item_count
FROM
    Inventory
    JOIN T
WHERE item_type = 'prime_eligible'
UNION ALL
SELECT
    'not_prime',
    IFNULL(COUNT(1) * FLOOR(IF(s = 0, 500000, 500000 % s) / SUM(square_footage)), 0)
FROM
    Inventory
    JOIN T
WHERE item_type = 'not_prime';
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
