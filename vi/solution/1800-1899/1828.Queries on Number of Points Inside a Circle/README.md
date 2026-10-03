---
comments: true
difficulty: Medium
rating: 1380
source: Biweekly Contest 50 Q2
tags:
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [1828. Queries on Number of Points Inside a Circle](https://leetcode.com/problems/queries-on-number-of-points-inside-a-circle)

[中文文档](/solution/1800-1899/1828.Queries%20on%20Number%20of%20Points%20Inside%20a%20Circle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> là tọa độ của điểm thứ <code>i<sup>th</sup></code> trên mặt phẳng 2D. Nhiều điểm có thể có <strong>cùng</strong> tọa độ.</p>

<p>Đồng thời, cho mảng <code>queries</code>, trong đó <code>queries[j] = [x<sub>j</sub>, y<sub>j</sub>, r<sub>j</sub>]</code> mô tả một đường tròn tâm tại <code>(x<sub>j</sub>, y<sub>j</sub>)</code> và bán kính <code>r<sub>j</sub></code>.</p>

<p>Với mỗi query <code>queries[j]</code>, hãy tính số điểm <strong>nằm trong</strong> đường tròn thứ <code>j<sup>th</sup></code>. Các điểm <strong>nằm trên đường biên</strong> đường tròn được xem là <strong>nằm trong</strong>.</p>

<p>Trả về <em>một mảng </em><code>answer</code><em>, trong đó </em><code>answer[j]</code><em> là đáp án của query thứ </em><code>j<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1828.Queries%20on%20Number%20of%20Points%20Inside%20a%20Circle/images/chrome_2021-03-25_22-34-16.png" style="width: 500px; height: 418px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,3],[3,3],[5,3],[2,2]], queries = [[2,3,1],[4,3,1],[1,1,2]]
<strong>Đầu ra:</strong> [3,2,2]
<b>Giải thích: </b>Các điểm và đường tròn được minh họa ở trên.
queries[0] là đường tròn màu xanh lá, queries[1] là đường tròn màu đỏ và queries[2] là đường tròn màu xanh dương.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1828.Queries%20on%20Number%20of%20Points%20Inside%20a%20Circle/images/chrome_2021-03-25_22-42-07.png" style="width: 500px; height: 390px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,1],[2,2],[3,3],[4,4],[5,5]], queries = [[1,2,2],[2,2,2],[4,3,2],[4,3,3]]
<strong>Đầu ra:</strong> [2,3,2,4]
<b>Giải thích: </b>Các điểm và đường tròn được minh họa ở trên.
queries[0] là đường tròn màu xanh lá, queries[1] màu đỏ, queries[2] màu xanh dương và queries[3] màu tím.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 500</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>​​​​​​i</sub>, y<sub>​​​​​​i</sub> &lt;= 500</code></li>
	<li><code>1 &lt;= queries.length &lt;= 500</code></li>
	<li><code>queries[j].length == 3</code></li>
	<li><code>0 &lt;= x<sub>j</sub>, y<sub>j</sub> &lt;= 500</code></li>
	<li><code>1 &lt;= r<sub>j</sub> &lt;= 500</code></li>
	<li>Tất cả tọa độ đều là số nguyên.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm đáp án cho mỗi query với độ phức tạp tốt hơn <code>O(n)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi query hỏi có bao nhiêu điểm đã cho nằm trong một đường tròn. Cả hai mảng có kích thước nhiều nhất $500$, và kiểm tra bình phương khoảng cách mất $O(1)$, nên liệt kê lồng nhau là đủ.
>
> Không cần chỉ mục không gian: với mỗi đường tròn, duyệt mọi điểm và kiểm tra $dx^2+dy^2\le r^2$ để tránh tính căn bậc hai. Chi phí $O(mn)$ phù hợp với giới hạn.

<!-- thinking:end -->

Liệt kê tất cả đường tròn $(x, y, r)$. Với mỗi đường tròn, tính số điểm nằm trong đường tròn để thu được đáp án.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là độ dài của các mảng `queries` và `points`. Không tính không gian lưu đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPoints(
        self, points: List[List[int]], queries: List[List[int]]
    ) -> List[int]:
        ans = []
        for x, y, r in queries:
            cnt = 0
            for i, j in points:
                dx, dy = i - x, j - y
                cnt += dx * dx + dy * dy <= r * r
            ans.append(cnt)
        return ans
```

#### Java

```java
class Solution {
    public int[] countPoints(int[][] points, int[][] queries) {
        int m = queries.length;
        int[] ans = new int[m];
        for (int k = 0; k < m; ++k) {
            int x = queries[k][0], y = queries[k][1], r = queries[k][2];
            for (var p : points) {
                int i = p[0], j = p[1];
                int dx = i - x, dy = j - y;
                if (dx * dx + dy * dy <= r * r) {
                    ++ans[k];
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countPoints(vector<vector<int>>& points, vector<vector<int>>& queries) {
        vector<int> ans;
        for (auto& q : queries) {
            int x = q[0], y = q[1], r = q[2];
            int cnt = 0;
            for (auto& p : points) {
                int i = p[0], j = p[1];
                int dx = i - x, dy = j - y;
                cnt += dx * dx + dy * dy <= r * r;
            }
            ans.emplace_back(cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func countPoints(points [][]int, queries [][]int) (ans []int) {
	for _, q := range queries {
		x, y, r := q[0], q[1], q[2]
		cnt := 0
		for _, p := range points {
			i, j := p[0], p[1]
			dx, dy := i-x, j-y
			if dx*dx+dy*dy <= r*r {
				cnt++
			}
		}
		ans = append(ans, cnt)
	}
	return
}
```

#### TypeScript

```ts
function countPoints(points: number[][], queries: number[][]): number[] {
    return queries.map(([cx, cy, r]) => {
        let res = 0;
        for (const [px, py] of points) {
            if (Math.sqrt((cx - px) ** 2 + (cy - py) ** 2) <= r) {
                res++;
            }
        }
        return res;
    });
}
```

#### Rust

```rust
impl Solution {
    pub fn count_points(points: Vec<Vec<i32>>, queries: Vec<Vec<i32>>) -> Vec<i32> {
        queries
            .iter()
            .map(|v| {
                let cx = v[0];
                let cy = v[1];
                let r = v[2].pow(2);
                let mut count = 0;
                for p in points.iter() {
                    if (p[0] - cx).pow(2) + (p[1] - cy).pow(2) <= r {
                        count += 1;
                    }
                }
                count
            })
            .collect()
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* countPoints(int** points, int pointsSize, int* pointsColSize, int** queries, int queriesSize, int* queriesColSize,
    int* returnSize) {
    int* ans = malloc(sizeof(int) * queriesSize);
    for (int i = 0; i < queriesSize; i++) {
        int cx = queries[i][0];
        int cy = queries[i][1];
        int r = queries[i][2];
        int count = 0;
        for (int j = 0; j < pointsSize; j++) {
            if (sqrt(pow(points[j][0] - cx, 2) + pow(points[j][1] - cy, 2)) <= r) {
                count++;
            }
        }
        ans[i] = count;
    }
    *returnSize = queriesSize;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
