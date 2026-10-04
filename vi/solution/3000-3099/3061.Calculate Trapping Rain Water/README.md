---
comments: true
difficulty: Hard
tags:
    - Database
---

<!-- problem:start -->

# [3061. Calculate Trapping Rain Water 🔒](https://leetcode.com/problems/calculate-trapping-rain-water)

[中文文档](/solution/3000-3099/3061.Calculate%20Trapping%20Rain%20Water/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <font face="monospace">Heights</font></p>

<pre>
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| height      | int  |
+-------------+------+
id là khóa chính (cột có các giá trị duy nhất) của bảng này và được đảm bảo có thứ tự liên tiếp.
Mỗi hàng của bảng này chứa một id và height.
</pre>

<p>Hãy viết một lời giải để tính lượng nước mưa có thể được <strong>giữ lại giữa các thanh</strong> trong địa hình, với giả sử mỗi thanh có <strong>chiều rộng</strong> là <code>1</code> đơn vị.</p>

<p><em>Trả về bảng kết quả theo </em><strong>bất kỳ</strong><em> thứ tự nào.</em></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Heights:
+-----+--------+
| id  | height |
+-----+--------+
| 1   | 0      |
| 2   | 1      |
| 3   | 0      |
| 4   | 2      |
| 5   | 1      |
| 6   | 0      |
| 7   | 1      |
| 8   | 3      |
| 9   | 2      |
| 10  | 1      |
| 11  | 2      |
| 12  | 1      |
+-----+--------+
<strong>Đầu ra:</strong>
+---------------------+
| total_trapped_water |
+---------------------+
| 6                   |
+---------------------+
<strong>Giải thích:</strong>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3061.Calculate%20Trapping%20Rain%20Water/images/trapping_rain_water.png" style="width:500px; height:200px;" />

Bản đồ độ cao được mô tả ở trên (trong phần màu đen) được biểu diễn bằng đồ thị, với trục x biểu thị id và trục y biểu thị các độ cao [0,1,0,2,1,0,1,3,2,1,2,1]. Trong trường hợp này, có 6 đơn vị nước mưa được giữ lại trong phần màu xanh.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ + Tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán trapping rain water kinh điển: mỗi ô chứa $\min(L,R)-h$. Với dữ liệu dạng bảng, có thể dùng giá trị lớn nhất lũy tiến thay cho two pointers.
>
> Giá trị lớn nhất lũy tiến từ trái và từ phải cho ta $l$ và $r$; chúng ta tính tổng $\min(l,r)-h$.
>
> $\texttt{cummax}$ và $\texttt{cummax}$ theo thứ tự đảo ngược thực hiện hai phía.

<!-- thinking:end -->

Chúng ta dùng hàm cửa sổ `MAX(height) OVER (ORDER BY id)` để tính chiều cao lớn nhất tại mỗi vị trí và ở phía bên trái, đồng thời dùng `MAX(height) OVER (ORDER BY id DESC)` để tính chiều cao lớn nhất tại mỗi vị trí và ở phía bên phải, lần lượt ký hiệu là `l` và `r`. Sau đó, lượng nước được giữ lại tại mỗi vị trí là `min(l, r) - height`. Cuối cùng, chúng ta tính tổng các lượng nước này.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT
            *,
            MAX(height) OVER (ORDER BY id) AS l,
            MAX(height) OVER (ORDER BY id DESC) AS r
        FROM Heights
    )
SELECT SUM(LEAST(l, r) - height) AS total_trapped_water
FROM T;
```

#### Python3

```python
import pandas as pd


def calculate_trapped_rain_water(heights: pd.DataFrame) -> pd.DataFrame:
    heights["l"] = heights["height"].cummax()
    heights["r"] = heights["height"][::-1].cummax()[::-1]
    heights["trapped_water"] = heights[["l", "r"]].min(axis=1) - heights["height"]
    return pd.DataFrame({"total_trapped_water": [heights["trapped_water"].sum()]})
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
