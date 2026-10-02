---
comments: true
difficulty: Easy
tags:
    - Geometry
    - Array
    - Math
    - Polygon
---

<!-- problem:start -->

# [812. Largest Triangle Area](https://leetcode.com/problems/largest-triangle-area)

[中文文档](/solution/0800-0899/0812.Largest%20Triangle%20Area/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các điểm <code>points</code> trên mặt phẳng <strong>X-Y</strong>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>. Hãy trả về <em>diện tích lớn nhất của tam giác tạo bởi ba điểm khác nhau bất kỳ</em>. Chấp nhận đáp án có sai số tối đa <code>10<sup>-5</sup></code> so với kết quả thực tế.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0812.Largest%20Triangle%20Area/images/1027.png" style="height: 369px; width: 450px;" />
<pre>
<strong>Đầu vào:</strong> points = [[0,0],[0,1],[1,0],[0,2],[2,0]]
<strong>Đầu ra:</strong> 2.00000
<strong>Giải thích:</strong> Năm điểm được thể hiện trong hình phía trên. Tam giác màu đỏ có diện tích lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[1,0],[0,0],[0,1]]
<strong>Đầu ra:</strong> 0.50000
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= points.length &lt;= 50</code></li>
	<li><code>-50 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 50</code></li>
	<li>Tất cả các điểm đã cho đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải: Liệt kê và áp dụng công thức diện tích tam giác

<!-- thinking:start -->

> **Tư duy**
>
> Diện tích tam giác bằng một nửa giá trị tuyệt đối của tích có hướng hai cạnh. Với tối đa $50$ điểm, có thể dùng ba vòng lặp lồng nhau.
>
> Liệt kê mọi bộ ba điểm, tính $|\mathbf{u}\times\mathbf{v}|/2$ rồi giữ giá trị lớn nhất. Ba điểm thẳng hàng cho diện tích $0$ nên không làm thay đổi đáp án.

<!-- thinking:end -->

Với ba điểm $(x_1, y_1)$, $(x_2, y_2)$, $(x_3, y_3)$ trên mặt phẳng, công thức tính diện tích là:

$$S = \frac{1}{2} \left| x_1y_2 + x_2y_3 + x_3y_1 - x_1y_3 - x_2y_1 - x_3y_2 \right|$$

Ta có thể liệt kê mọi tổ hợp gồm ba điểm rồi tính diện tích lớn nhất.

Độ phức tạp thời gian là $O(n^3)$, trong đó $n$ là số điểm. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestTriangleArea(self, points: List[List[int]]) -> float:
        ans = 0
        for x1, y1 in points:
            for x2, y2 in points:
                for x3, y3 in points:
                    u1, v1 = x2 - x1, y2 - y1
                    u2, v2 = x3 - x1, y3 - y1
                    t = abs(u1 * v2 - u2 * v1) / 2
                    ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public double largestTriangleArea(int[][] points) {
        double ans = 0;
        for (int[] p1 : points) {
            int x1 = p1[0], y1 = p1[1];
            for (int[] p2 : points) {
                int x2 = p2[0], y2 = p2[1];
                for (int[] p3 : points) {
                    int x3 = p3[0], y3 = p3[1];
                    int u1 = x2 - x1, v1 = y2 - y1;
                    int u2 = x3 - x1, v2 = y3 - y1;
                    double t = Math.abs(u1 * v2 - u2 * v1) / 2.0;
                    ans = Math.max(ans, t);
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
    double largestTriangleArea(vector<vector<int>>& points) {
        double ans = 0;
        for (auto& p1 : points) {
            int x1 = p1[0], y1 = p1[1];
            for (auto& p2 : points) {
                int x2 = p2[0], y2 = p2[1];
                for (auto& p3 : points) {
                    int x3 = p3[0], y3 = p3[1];
                    int u1 = x2 - x1, v1 = y2 - y1;
                    int u2 = x3 - x1, v2 = y3 - y1;
                    double t = abs(u1 * v2 - u2 * v1) / 2.0;
                    ans = max(ans, t);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestTriangleArea(points [][]int) float64 {
	ans := 0.0
	for _, p1 := range points {
		x1, y1 := p1[0], p1[1]
		for _, p2 := range points {
			x2, y2 := p2[0], p2[1]
			for _, p3 := range points {
				x3, y3 := p3[0], p3[1]
				u1, v1 := x2-x1, y2-y1
				u2, v2 := x3-x1, y3-y1
				t := float64(abs(u1*v2-u2*v1)) / 2.0
				ans = math.Max(ans, t)
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
function largestTriangleArea(points: number[][]): number {
    let ans = 0;
    for (const [x1, y1] of points) {
        for (const [x2, y2] of points) {
            for (const [x3, y3] of points) {
                const u1 = x2 - x1,
                    v1 = y2 - y1;
                const u2 = x3 - x1,
                    v2 = y3 - y1;
                const t = Math.abs(u1 * v2 - u2 * v1) / 2;
                ans = Math.max(ans, t);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn largest_triangle_area(points: Vec<Vec<i32>>) -> f64 {
        let mut ans: f64 = 0.0;
        for point1 in &points {
            let (x1, y1) = (point1[0], point1[1]);
            for point2 in &points {
                let (x2, y2) = (point2[0], point2[1]);
                for point3 in &points {
                    let (x3, y3) = (point3[0], point3[1]);
                    let u1 = x2 - x1;
                    let v1 = y2 - y1;
                    let u2 = x3 - x1;
                    let v2 = y3 - y1;
                    let t = ((u1 * v2 - u2 * v1) as f64).abs() / 2.0;
                    ans = ans.max(t);
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
