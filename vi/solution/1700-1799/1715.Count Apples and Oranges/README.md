---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [1715. Count Apples and Oranges 🔒](https://leetcode.com/problems/count-apples-and-oranges)

[中文文档](/solution/1700-1799/1715.Count%20Apples%20and%20Oranges/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Boxes</code></p>

<pre>
+--------------+------+
| Column Name  | Type |
+--------------+------+
| box_id       | int  |
| chest_id     | int  |
| apple_count  | int  |
| orange_count | int  |
+--------------+------+
box_id là cột có các giá trị duy nhất trong bảng này.
chest_id là khóa ngoại (cột tham chiếu) của bảng chests.
Bảng này chứa thông tin về các hộp cùng số táo và cam bên trong. Mỗi hộp có thể chứa một rương, và rương cũng có thể chứa táo và cam.
</pre>

<p>&nbsp;</p>

<p>Bảng: <code>Chests</code></p>

<pre>
+--------------+------+
| Column Name  | Type |
+--------------+------+
| chest_id     | int  |
| apple_count  | int  |
| orange_count | int  |
+--------------+------+
chest_id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa thông tin về các rương cùng số táo và cam tương ứng.
</pre>

<p>&nbsp;</p>

<p>Hãy viết lời giải để đếm số táo và cam trong tất cả các hộp. Nếu một hộp chứa rương, cũng phải cộng số táo và cam trong rương đó.</p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong>
Boxes table:
+--------+----------+-------------+--------------+
| box_id | chest_id | apple_count | orange_count |
+--------+----------+-------------+--------------+
| 2      | null     | 6           | 15           |
| 18     | 14       | 4           | 15           |
| 19     | 3        | 8           | 4            |
| 12     | 2        | 19          | 20           |
| 20     | 6        | 12          | 9            |
| 8      | 6        | 9           | 9            |
| 3      | 14       | 16          | 7            |
+--------+----------+-------------+--------------+
Chests table:
+----------+-------------+--------------+
| chest_id | apple_count | orange_count |
+----------+-------------+--------------+
| 6        | 5           | 6            |
| 14       | 20          | 10           |
| 2        | 8           | 8            |
| 3        | 19          | 4            |
| 16       | 19          | 19           |
+----------+-------------+--------------+
<strong>Output:</strong>
+-------------+--------------+
| apple_count | orange_count |
+-------------+--------------+
| 151         | 123          |
+-------------+--------------+
<strong>Giải thích:</strong>
hộp 2 có 6 quả táo và 15 quả cam.
hộp 18 có 4 + 20 (từ rương) = 24 quả táo và 15 + 10 (từ rương) = 25 quả cam.
hộp 19 có 8 + 19 (từ rương) = 27 quả táo và 4 + 4 (từ rương) = 8 quả cam.
hộp 12 có 19 + 8 (từ rương) = 27 quả táo và 20 + 8 (từ rương) = 28 quả cam.
hộp 20 có 12 + 5 (từ rương) = 17 quả táo và 9 + 6 (từ rương) = 15 quả cam.
hộp 8 có 9 + 5 (từ rương) = 14 quả táo và 9 + 6 (từ rương) = 15 quả cam.
hộp 3 có 16 + 20 (từ rương) = 36 quả táo và 7 + 10 (từ rương) = 17 quả cam.
Tổng số táo = 6 + 24 + 27 + 27 + 17 + 14 + 36 = 151
Tổng số cam = 15 + 25 + 8 + 28 + 15 + 15 + 17 = 123
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Left Join + Summation

<!-- thinking:start -->

> **Tư duy**
>
> Một hộp có thể chứa rương, nên táo và cam đến từ cả hai nơi. Nếu không có rương thì phần đóng góp phải bằng 0.
>
> Left join $\textit{Boxes}$ với $\textit{Chests}$ theo $\textit{chest\_id}$, thay giá trị null bằng $0$, rồi cộng các cột táo và cam.

<!-- thinking:end -->

Ta có thể left join bảng `Boxes` và `Chests` dựa trên `chest_id`, rồi lần lượt tính tổng số táo và cam. Nếu một hộp không chứa rương thì `chest_id` tương ứng là `null`. Khi đó, số táo và cam trong rương của hộp được xem là 0.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
SELECT
    SUM(IFNULL(b.apple_count, 0) + IFNULL(c.apple_count, 0)) AS apple_count,
    SUM(IFNULL(b.orange_count, 0) + IFNULL(c.orange_count, 0)) AS orange_count
FROM
    Boxes AS b
    LEFT JOIN Chests AS c USING (chest_id);
```

#### Pandas

```python
import pandas as pd


def count_apples_and_oranges(boxes: pd.DataFrame, chests: pd.DataFrame) -> pd.DataFrame:
    merged_df = boxes.merge(
        chests, on="chest_id", how="left", suffixes=("_box", "_chest")
    )
    apple_count = (
        merged_df["apple_count_box"].fillna(0)
        + merged_df["apple_count_chest"].fillna(0)
    ).sum()
    orange_count = (
        merged_df["orange_count_box"].fillna(0)
        + merged_df["orange_count_chest"].fillna(0)
    ).sum()
    return pd.DataFrame({"apple_count": [apple_count], "orange_count": [orange_count]})
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
