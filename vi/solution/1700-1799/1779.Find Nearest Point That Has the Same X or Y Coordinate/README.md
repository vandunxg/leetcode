---
comments: true
difficulty: Easy
rating: 1259
source: Biweekly Contest 47 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1779. Find Nearest Point That Has the Same X or Y Coordinate](https://leetcode.com/problems/find-nearest-point-that-has-the-same-x-or-y-coordinate)

[中文文档](/solution/1700-1799/1779.Find%20Nearest%20Point%20That%20Has%20the%20Same%20X%20or%20Y%20Coordinate/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>x</code> và <code>y</code> biểu diễn vị trí hiện tại của bạn trên mặt phẳng Descartes: <code>(x, y)</code>. Ngoài ra, cho mảng <code>points</code>, trong đó mỗi <code>points[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu diễn một điểm tại <code>(a<sub>i</sub>, b<sub>i</sub>)</code>. Một điểm <strong>hợp lệ</strong> nếu có cùng hoành độ hoặc cùng tung độ với vị trí của bạn.</p>

<p>Trả về <em>chỉ số <strong>(đánh số từ 0)</strong> của điểm <strong>hợp lệ</strong> có <strong>khoảng cách Manhattan</strong> nhỏ nhất đến vị trí hiện tại</em>. Nếu có nhiều điểm, trả về <em>điểm hợp lệ có chỉ số <strong>nhỏ nhất</strong></em>. Nếu không có điểm hợp lệ, trả về <code>-1</code>.</p>

<p><strong>Khoảng cách Manhattan</strong> giữa hai điểm <code>(x<sub>1</sub>, y<sub>1</sub>)</code> và <code>(x<sub>2</sub>, y<sub>2</sub>)</code> là <code>abs(x<sub>1</sub> - x<sub>2</sub>) + abs(y<sub>1</sub> - y<sub>2</sub>)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> x = 3, y = 4, points = [[1,2],[3,1],[2,4],[2,3],[4,4]]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Trong tất cả các điểm, chỉ [3,1], [2,4] và [4,4] là hợp lệ. Trong các điểm hợp lệ, [2,4] và [4,4] có khoảng cách Manhattan nhỏ nhất đến vị trí hiện tại, bằng 1. [2,4] có chỉ số nhỏ hơn, nên trả về 2.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> x = 3, y = 4, points = [[3,4]]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Điểm được chọn có thể nằm ngay tại vị trí hiện tại.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> x = 3, y = 4, points = [[2,3]]
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Không có điểm hợp lệ.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 10<sup>4</sup></code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>1 &lt;= x, y, a<sub>i</sub>, b<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Điểm hợp lệ có cùng tọa độ $x$ hoặc $y$. Ta cần chỉ số nhỏ nhất trong các điểm có khoảng cách Manhattan nhỏ nhất. Với $n\le 10^4$, chỉ cần duyệt một lần.
>
> Với mỗi điểm hợp lệ, tính $|a-x|+|b-y|$ rồi lưu khoảng cách và chỉ số tốt nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nearestValidPoint(self, x: int, y: int, points: List[List[int]]) -> int:
        ans, mi = -1, inf
        for i, (a, b) in enumerate(points):
            if a == x or b == y:
                d = abs(a - x) + abs(b - y)
                if mi > d:
                    ans, mi = i, d
        return ans
```

#### Java

```java
class Solution {
    public int nearestValidPoint(int x, int y, int[][] points) {
        int ans = -1, mi = 1000000;
        for (int i = 0; i < points.length; ++i) {
            int a = points[i][0], b = points[i][1];
            if (a == x || b == y) {
                int d = Math.abs(a - x) + Math.abs(b - y);
                if (d < mi) {
                    mi = d;
                    ans = i;
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
    int nearestValidPoint(int x, int y, vector<vector<int>>& points) {
        int ans = -1, mi = 1e6;
        for (int i = 0; i < points.size(); ++i) {
            int a = points[i][0], b = points[i][1];
            if (a == x || b == y) {
                int d = abs(a - x) + abs(b - y);
                if (d < mi) {
                    mi = d;
                    ans = i;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func nearestValidPoint(x int, y int, points [][]int) int {
	ans, mi := -1, 1000000
	for i, p := range points {
		a, b := p[0], p[1]
		if a == x || b == y {
			d := abs(a-x) + abs(b-y)
			if d < mi {
				ans, mi = i, d
			}
		}
	}
	return ans
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
function nearestValidPoint(x: number, y: number, points: number[][]): number {
    let res = -1;
    let midDif = Infinity;
    points.forEach(([px, py], i) => {
        if (px != x && py != y) {
            return;
        }
        const dif = Math.abs(px - x) + Math.abs(py - y);
        if (dif < midDif) {
            midDif = dif;
            res = i;
        }
    });
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn nearest_valid_point(x: i32, y: i32, points: Vec<Vec<i32>>) -> i32 {
        let n = points.len();
        let mut min_dif = i32::MAX;
        let mut res = -1;
        for i in 0..n {
            let (p_x, p_y) = (points[i][0], points[i][1]);
            if p_x != x && p_y != y {
                continue;
            }
            let dif = (p_x - x).abs() + (p_y - y).abs();
            if dif < min_dif {
                min_dif = dif;
                res = i as i32;
            }
        }
        res
    }
}
```

#### C

```c
int nearestValidPoint(int x, int y, int** points, int pointsSize, int* pointsColSize) {
    int ans = -1;
    int min = INT_MAX;
    for (int i = 0; i < pointsSize; i++) {
        int* point = points[i];
        if (point[0] != x && point[1] != y) {
            continue;
        }
        int d = abs(x - point[0]) + abs(y - point[1]);
        if (d < min) {
            min = d;
            ans = i;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
