---
comments: true
difficulty: Easy
rating: 1302
source: Weekly Contest 164 Q1
tags:
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [1266. Minimum Time Visiting All Points](https://leetcode.com/problems/minimum-time-visiting-all-points)

[中文文档](/solution/1200-1299/1266.Minimum%20Time%20Visiting%20All%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Trên mặt phẳng 2D có <code>n</code> điểm với tọa độ nguyên <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>. Hãy trả về <em><strong>thời gian tối thiểu</strong> tính bằng giây để đi qua tất cả điểm theo thứ tự trong </em><code>points</code>.</p>

<p>Bạn có thể di chuyển theo các quy tắc sau:</p>

<ul>
	<li>Trong <code>1</code> giây, bạn có thể:

    <ul>
    	<li>di chuyển theo chiều dọc một&nbsp;đơn vị,</li>
    	<li>di chuyển theo chiều ngang một đơn vị, hoặc</li>
    	<li>di chuyển chéo một khoảng <code>sqrt(2)</code> đơn vị (tức là trong <code>1</code> giây, đi một đơn vị theo chiều dọc và một đơn vị theo chiều ngang).</li>
    </ul>
    </li>
    <li>Bạn phải đi qua các điểm theo đúng thứ tự xuất hiện trong mảng.</li>
    <li>Bạn có thể đi qua những điểm xuất hiện sau đó trong thứ tự, nhưng lượt đi qua này không được tính là đã ghé thăm điểm.</li>

</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1266.Minimum%20Time%20Visiting%20All%20Points/images/1626_example_1.png" style="width: 500px; height: 428px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,1],[3,4],[-1,0]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích: </strong>Một lộ trình tối ưu là <strong>[1,1]</strong> -&gt; [2,2] -&gt; [3,3] -&gt; <strong>[3,4] </strong>-&gt; [2,3] -&gt; [1,2] -&gt; [0,1] -&gt; <strong>[-1,0]</strong>   
Thời gian đi từ [1,1] đến [3,4] = 3 giây 
Thời gian đi từ [3,4] đến [-1,0] = 4 giây
Tổng thời gian = 7 giây</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[3,2],[-2,2]]
<strong>Đầu ra:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>points.length == n</code></li>
	<li><code>1 &lt;= n&nbsp;&lt;= 100</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>-1000&nbsp;&lt;= points[i][0], points[i][1]&nbsp;&lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giây có thể di chuyển theo tám hướng, nên thời gian giữa hai điểm liên tiếp bằng khoảng cách Chebyshev $\max(|\Delta x|,|\Delta y|)$: mỗi bước chéo đồng thời rút ngắn cả hai độ lệch. Các điểm được ghé thăm theo thứ tự, vì vậy tổng thời gian là tổng các khoảng cách này. Với $n \le 100$, chỉ cần một lượt duyệt tuyến tính.

<!-- thinking:end -->

Với hai điểm $p_1=(x_1, y_1)$ và $p_2=(x_2, y_2)$, khoảng cách theo chiều ngang và chiều dọc lần lượt là $d_x = |x_1 - x_2|$ và $d_y = |y_1 - y_2|$.

Nếu $d_x \ge d_y$, ta di chuyển chéo $d_y$ bước rồi đi ngang thêm $d_x - d_y$ bước; nếu $d_x < d_y$, ta di chuyển chéo $d_x$ bước rồi đi dọc thêm $d_y - d_x$ bước. Vì vậy, thời gian ngắn nhất giữa hai điểm là $\max(d_x, d_y)$.

Duyệt lần lượt các cặp điểm liên tiếp, tính thời gian ngắn nhất giữa từng cặp rồi cộng lại.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số điểm. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTimeToVisitAllPoints(self, points: List[List[int]]) -> int:
        return sum(
            max(abs(p1[0] - p2[0]), abs(p1[1] - p2[1])) for p1, p2 in pairwise(points)
        )
```

#### Java

```java
class Solution {
    public int minTimeToVisitAllPoints(int[][] points) {
        int ans = 0;
        for (int i = 1; i < points.length; ++i) {
            int dx = Math.abs(points[i][0] - points[i - 1][0]);
            int dy = Math.abs(points[i][1] - points[i - 1][1]);
            ans += Math.max(dx, dy);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minTimeToVisitAllPoints(vector<vector<int>>& points) {
        int ans = 0;
        for (int i = 1; i < points.size(); ++i) {
            int dx = abs(points[i][0] - points[i - 1][0]);
            int dy = abs(points[i][1] - points[i - 1][1]);
            ans += max(dx, dy);
        }
        return ans;
    }
};
```

#### Go

```go
func minTimeToVisitAllPoints(points [][]int) (ans int) {
	for i, p := range points[1:] {
		dx := abs(p[0] - points[i][0])
		dy := abs(p[1] - points[i][1])
		ans += max(dx, dy)
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minTimeToVisitAllPoints(points: number[][]): number {
    let ans = 0;
    for (let i = 1; i < points.length; i++) {
        const dx = Math.abs(points[i][0] - points[i - 1][0]);
        const dy = Math.abs(points[i][1] - points[i - 1][1]);
        ans += Math.max(dx, dy);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_time_to_visit_all_points(points: Vec<Vec<i32>>) -> i32 {
        let mut ans = 0;
        for i in 1..points.len() {
            let dx = (points[i][0] - points[i - 1][0]).abs();
            let dy = (points[i][1] - points[i - 1][1]).abs();
            ans += dx.max(dy);
        }
        ans
    }
}
```

#### C

```c
#define max(a, b) ((a) > (b) ? (a) : (b))

int minTimeToVisitAllPoints(int** points, int pointsSize, int* pointsColSize) {
    int ans = 0;
    for (int i = 1; i < pointsSize; ++i) {
        int dx = abs(points[i][0] - points[i - 1][0]);
        int dy = abs(points[i][1] - points[i - 1][1]);
        ans += max(dx, dy);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
