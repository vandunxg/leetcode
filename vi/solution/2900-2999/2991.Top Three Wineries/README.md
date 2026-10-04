---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [2991. Top Three Wineries 🔒](https://leetcode.com/problems/top-three-wineries)

[中文文档](/solution/2900-2999/2991.Top%20Three%20Wineries/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code>Wineries</code></p>

<pre>
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| country     | varchar  |
| points      | int      |
| winery      | varchar  |
+-------------+----------+
id là cột có các giá trị duy nhất trong bảng này.
Bảng này chứa id, country, points và winery.
</pre>

<p>Hãy viết lời giải để tìm <strong>ba winery hàng đầu</strong> ở <strong>mỗi</strong> <strong>quốc gia</strong> dựa trên <strong>tổng điểm</strong> của chúng. Nếu <strong>nhiều winery</strong> có <strong>cùng</strong> tổng điểm, hãy sắp xếp chúng theo tên <code>winery</code> theo thứ tự <strong>tăng dần</strong>. Nếu <strong>không có winery thứ hai</strong>, hãy xuất &#39;No second winery,&#39;, và nếu <strong>không có winery thứ ba</strong>, hãy xuất &#39;No third winery.&#39;.</p>

<p><em>Trả về bảng kết quả được sắp xếp theo </em><code>country</code><em> theo thứ tự <strong>tăng dần</strong></em><em>.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Wineries table:
+-----+-----------+--------+-----------------+
| id  | country   | points | winery          |
+-----+-----------+--------+-----------------+
| 103 | Australia | 84     | WhisperingPines |
| 737 | Australia | 85     | GrapesGalore    |
| 848 | Australia | 100    | HarmonyHill     |
| 222 | Hungary   | 60     | MoonlitCellars  |
| 116 | USA       | 47     | RoyalVines      |
| 124 | USA       | 45     | Eagle&#39;sNest     |
| 648 | India     | 69     | SunsetVines     |
| 894 | USA       | 39     | RoyalVines      |
| 677 | USA       | 9      | PacificCrest    |
+-----+-----------+--------+-----------------+
<strong>Đầu ra:</strong>
+-----------+---------------------+-------------------+----------------------+
| country   | top_winery          | second_winery     | third_winery         |
+-----------+---------------------+-------------------+----------------------+
| Australia | HarmonyHill (100)   | GrapesGalore (85) | WhisperingPines (84) |
| Hungary   | MoonlitCellars (60) | No second winery  | No third winery      |
| India     | SunsetVines (69)    | No second winery  | No third winery      |
| USA       | RoyalVines (86)     | Eagle&#39;sNest (45)  | PacificCrest (9)     |
+-----------+---------------------+-------------------+----------------------+
<strong>Giải thích</strong>
Đối với Australia
 - Winery HarmonyHill đạt tổng điểm cao nhất là 100 điểm tại Australia.
 - Winery GrapesGalore có tổng cộng 85 điểm, đứng thứ hai tại Australia.
 - Winery WhisperingPines có tổng cộng 80 điểm, đứng thứ ba.
Đối với Hungary
 - MoonlitCellars là winery duy nhất, đạt 60 điểm, nên đương nhiên đứng đầu. Không có winery thứ hai hoặc thứ ba.
Đối với India
 - SunsetVines là winery duy nhất, đạt 69 điểm, nên đứng đầu. Không có winery thứ hai hoặc thứ ba.
Đối với USA
 - RoyalVines Wines đạt tổng cộng 47 + 39 = 86 điểm, đứng đầu tại USA.
 - Eagle&#39;sNest có tổng cộng 45 điểm, đứng thứ hai tại USA.
 - PacificCrest đạt 9 điểm, đứng thứ ba tại USA
Bảng kết quả được sắp xếp theo country theo thứ tự tăng dần.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhóm dữ liệu + Window Function + Left Join

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi quốc gia, cần tìm ba winery có tổng điểm cao nhất, sắp xếp theo điểm rồi đến tên, đồng thời điền giá trị thay thế khi có ít hơn ba winery. Trước tiên, tính tổng theo từng quốc gia và winery, sau đó dùng $RANK$.
>
> Ba phép left join ghép các dòng có $rk=1,2,3$; $IFNULL$ điền các vị trí còn thiếu. Cuối cùng, sắp xếp theo quốc gia.

<!-- thinking:end -->

Trước tiên, ta có thể nhóm bảng `Wineries` theo `country` và `winery`, tính tổng điểm `points` cho mỗi nhóm. Sau đó, dùng window function `RANK()` để tiếp tục nhóm dữ liệu theo `country`, sắp xếp `points` theo thứ tự giảm dần và `winery` theo thứ tự tăng dần, rồi dùng hàm `CONCAT()` nối `winery` và `points`, tạo ra dữ liệu sau đây, được ký hiệu là bảng `T`:

| country   | winery               | rk  |
| --------- | -------------------- | --- |
| Australia | HarmonyHill (100)    | 1   |
| Australia | GrapesGalore (85)    | 2   |
| Australia | WhisperingPines (84) | 3   |
| Hungary   | MoonlitCellars (60)  | 1   |
| India     | SunsetVines (69)     | 1   |
| USA       | RoyalVines (86)      | 1   |
| USA       | Eagle'sNest (45)     | 2   |
| USA       | PacificCrest (9)     | 3   |

Tiếp theo, chỉ cần lọc dữ liệu có `rk = 1`, rồi nối bảng `T` với chính nó hai lần, lần lượt kết nối dữ liệu có `rk = 2` và `rk = 3`, để nhận kết quả cuối cùng.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            country,
            CONCAT(winery, ' (', points, ')') AS winery,
            RANK() OVER (
                PARTITION BY country
                ORDER BY points DESC, winery
            ) AS rk
        FROM (SELECT country, SUM(points) AS points, winery FROM Wineries GROUP BY 1, 3) AS t
    )
SELECT
    t1.country,
    t1.winery AS top_winery,
    IFNULL(t2.winery, 'No second winery') AS second_winery,
    IFNULL(t3.winery, 'No third winery') AS third_winery
FROM
    T AS t1
    LEFT JOIN T AS t2 ON t1.country = t2.country AND t1.rk = t2.rk - 1
    LEFT JOIN T AS t3 ON t2.country = t3.country AND t2.rk = t3.rk - 1
WHERE t1.rk = 1
ORDER BY 1;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
