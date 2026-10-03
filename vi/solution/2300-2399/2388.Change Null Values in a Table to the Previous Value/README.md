---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [2388. Change Null Values in a Table to the Previous Value 🔒](https://leetcode.com/problems/change-null-values-in-a-table-to-the-previous-value)

[中文文档](/solution/2300-2399/2388.Change%20Null%20Values%20in%20a%20Table%20to%20the%20Previous%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>CoffeeShop</code></p>

<pre>
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| drink       | varchar |
+-------------+---------+
id là khóa chính (cột có các giá trị duy nhất) của bảng này.
Mỗi hàng trong bảng này cho biết ID của đơn hàng và tên đồ uống đã đặt. Một số hàng drink có giá trị null.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để thay thế các giá trị <code>null</code> của drink bằng tên đồ uống của hàng trước đó có giá trị khác <code>null</code>. Đảm bảo rằng drink ở hàng đầu tiên của bảng không phải là <code>null</code>.</p>

<p>Trả về bảng kết quả <strong>theo cùng thứ tự với đầu vào</strong>.</p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng CoffeeShop:
+----+-------------------+
| id | drink             |
+----+-------------------+
| 9  | Rum and Coke      |
| 6  | null              |
| 7  | null              |
| 3  | St Germain Spritz |
| 1  | Orange Margarita  |
| 2  | null              |
+----+-------------------+
<strong>Đầu ra:</strong>
+----+-------------------+
| id | drink             |
+----+-------------------+
| 9  | Rum and Coke      |
| 6  | Rum and Coke      |
| 7  | Rum and Coke      |
| 3  | St Germain Spritz |
| 1  | Orange Margarita  |
| 2  | Orange Margarita  |
+----+-------------------+
<strong>Giải thích:</strong>
Đối với ID 6, giá trị trước đó khác null là của ID 9. Ta thay thế giá trị null bằng &quot;Rum and Coke&quot;.
Đối với ID 7, giá trị trước đó khác null là của ID 9. Ta thay thế giá trị null bằng &quot;Rum and Coke;.
Đối với ID 2, giá trị trước đó khác null là của ID 1. Ta thay thế giá trị null bằng &quot;Orange Margarita&quot;.
Lưu ý rằng các hàng trong đầu ra giống với đầu vào.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một giá trị null của drink cần được thay bằng giá trị khác null gần nhất trước đó theo thứ tự của bảng. Một biến session lưu giá trị đó trong quá trình quét.
>
> Khi gặp giá trị khác null, gán $@cur$; khi gặp null, giữ nguyên $@cur$. Chiếu giá trị đó thành cột mới.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    id,
    CASE
        WHEN drink IS NOT NULL THEN @cur := drink
        ELSE @cur
    END AS drink
FROM CoffeeShop;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Các biến session kém portable hơn. $ROW\_NUMBER$ cố định thứ tự; phép tính tổng lũy kế các cờ khác null tạo thành các nhóm; $MAX(drink)$ trong mỗi nhóm chính là giá trị khác null duy nhất của nhóm đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    S AS (
        SELECT *, ROW_NUMBER() OVER () AS rk
        FROM CoffeeShop
    ),
    T AS (
        SELECT
            *,
            SUM(
                CASE
                    WHEN drink IS NULL THEN 0
                    ELSE 1
                END
            ) OVER (ORDER BY rk) AS gid
        FROM S
    )
SELECT
    id,
    MAX(drink) OVER (
        PARTITION BY gid
        ORDER BY rk
    ) AS drink
FROM T;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
