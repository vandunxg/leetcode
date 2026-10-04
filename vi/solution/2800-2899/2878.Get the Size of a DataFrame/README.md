---
comments: true
difficulty: Easy
tags:
    - Pandas
---

<!-- problem:start -->

# [2878. Get the Size of a DataFrame](https://leetcode.com/problems/get-the-size-of-a-dataframe)

[中文文档](/solution/2800-2899/2878.Get%20the%20Size%20of%20a%20DataFrame/README.md)

## Mô tả

<!-- description:start -->

<pre>
DataFrame <code>players:</code>
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| player_id   | int    |
| name        | object |
| age         | int    |
| position    | object |
| ...         | ...    |
+-------------+--------+
</pre>

<p>Viết một lời giải để tính và hiển thị <strong>số hàng và số cột</strong> của <code>players</code>.</p>

<p>Trả về kết quả dưới dạng một mảng:</p>

<p><code>[number of rows, number of columns]</code></p>

<p>Định dạng kết quả được minh họa trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>+-----------+----------+-----+-------------+--------------------+
| player_id | name     | age | position    | team               |
+-----------+----------+-----+-------------+--------------------+
| 846       | Mason    | 21  | Forward     | RealMadrid         |
| 749       | Riley    | 30  | Winger      | Barcelona          |
| 155       | Bob      | 28  | Striker     | ManchesterUnited   |
| 583       | Isabella | 32  | Goalkeeper  | Liverpool          |
| 388       | Zachary  | 24  | Midfielder  | BayernMunich       |
| 883       | Ava      | 23  | Defender    | Chelsea            |
| 355       | Violet   | 18  | Striker     | Juventus           |
| 247       | Thomas   | 27  | Striker     | ParisSaint-Germain |
| 761       | Jack     | 33  | Midfielder  | ManchesterCity     |
| 642       | Charlie  | 36  | Center-back | Arsenal            |
+-----------+----------+-----+-------------+--------------------+<strong>
Đầu ra:
</strong>[10, 5]
<strong>Giải thích:</strong>
DataFrame này chứa 10 hàng và 5 cột.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị cần trả về là một cặp gồm số hàng và số cột. `shape` đã lưu sẵn cả hai giá trị; chỉ cần chuyển nó thành một danh sách là đủ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
import pandas as pd


def getDataframeSize(players: pd.DataFrame) -> List[int]:
    return list(players.shape)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
