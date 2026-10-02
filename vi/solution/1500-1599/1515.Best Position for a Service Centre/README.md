---
comments: true
difficulty: Hard
rating: 2156
source: Weekly Contest 197 Q4
tags:
    - Geometry
    - Array
    - Math
    - Randomized
---

<!-- problem:start -->

# [1515. Best Position for a Service Centre](https://leetcode.com/problems/best-position-for-a-service-centre)

[中文文档](/solution/1500-1599/1515.Best%20Position%20for%20a%20Service%20Centre/README.md)

## Mô tả

<!-- description:start -->

<p>Một công ty giao hàng muốn xây dựng trung tâm dịch vụ mới trong một thành phố. Công ty biết vị trí của mọi khách hàng trên bản đồ 2D và muốn xây trung tâm tại vị trí sao cho <strong>tổng khoảng cách Euclid đến mọi khách hàng là nhỏ nhất</strong>.</p>

<p>Với mảng <code>positions</code>, trong đó <code>positions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> là vị trí của khách hàng thứ <code>ith</code> trên bản đồ, hãy trả về <em>tổng khoảng cách Euclid nhỏ nhất</em> đến mọi khách hàng.</p>

<p>Nói cách khác, cần chọn vị trí của trung tâm dịch vụ <code>[x<sub>centre</sub>, y<sub>centre</sub>]</code> sao cho công thức sau đạt giá trị nhỏ nhất:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1515.Best%20Position%20for%20a%20Service%20Centre/images/q4_edited.jpg" />
<p>Các đáp án sai khác không quá <code>10<sup>-5</sup></code> so với giá trị thực sẽ được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1515.Best%20Position%20for%20a%20Service%20Centre/images/q4_e1.jpg" style="width: 377px; height: 362px;" />
<pre>
<strong>Input:</strong> positions = [[0,1],[1,0],[1,2],[2,1]]
<strong>Output:</strong> 4.00000
<strong>Explanation:</strong> Như hình minh họa, chọn [x<sub>centre</sub>, y<sub>centre</sub>] = [1, 1] khiến khoảng cách đến mỗi khách hàng bằng 1; tổng các khoảng cách là 4, đây là giá trị nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1515.Best%20Position%20for%20a%20Service%20Centre/images/q4_e3.jpg" style="width: 419px; height: 419px;" />
<pre>
<strong>Input:</strong> positions = [[1,1],[3,3]]
<strong>Output:</strong> 2.82843
<strong>Explanation:</strong> Tổng khoảng cách nhỏ nhất có thể có = sqrt(2) + sqrt(2) = 2.82843
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= positions.length &lt;= 50</code></li>
	<li><code>positions[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Geometric median làm nhỏ nhất tổng khoảng cách Euclid đến các khách hàng. Hàm mục tiêu khả vi trên mặt phẳng nhưng không có công thức đóng đơn giản. Kích thước dữ liệu đủ nhỏ để xấp xỉ lặp với sai số trong $10^{-5}$.
>
> Bắt đầu tại trọng tâm và đi theo hướng gradient, là tổng các vector đơn vị hướng về khách hàng. Giảm learning rate theo hệ số $0.999$, đồng thời thêm một số rất nhỏ vào mẫu để tránh chia cho 0 khi đứng tại vị trí khách hàng. Dừng khi cả hai thành phần bước đi đều nhỏ hơn $10^{-6}$ và trả về tổng khoảng cách hiện tại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMinDistSum(self, positions: List[List[int]]) -> float:
        n = len(positions)
        x = y = 0
        for x1, y1 in positions:
            x += x1
            y += y1
        x, y = x / n, y / n
        decay = 0.999
        eps = 1e-6
        alpha = 0.5
        while 1:
            grad_x = grad_y = 0
            dist = 0
            for x1, y1 in positions:
                a = x - x1
                b = y - y1
                c = sqrt(a * a + b * b)
                grad_x += a / (c + 1e-8)
                grad_y += b / (c + 1e-8)
                dist += c
            dx = grad_x * alpha
            dy = grad_y * alpha
            x -= dx
            y -= dy
            alpha *= decay
            if abs(dx) <= eps and abs(dy) <= eps:
                return dist
```

#### Java

```java
class Solution {
    public double getMinDistSum(int[][] positions) {
        int n = positions.length;
        double x = 0, y = 0;
        for (int[] p : positions) {
            x += p[0];
            y += p[1];
        }
        x /= n;
        y /= n;
        double decay = 0.999;
        double eps = 1e-6;
        double alpha = 0.5;
        while (true) {
            double gradX = 0, gradY = 0;
            double dist = 0;
            for (int[] p : positions) {
                double a = x - p[0], b = y - p[1];
                double c = Math.sqrt(a * a + b * b);
                gradX += a / (c + 1e-8);
                gradY += b / (c + 1e-8);
                dist += c;
            }
            double dx = gradX * alpha, dy = gradY * alpha;
            if (Math.abs(dx) <= eps && Math.abs(dy) <= eps) {
                return dist;
            }
            x -= dx;
            y -= dy;
            alpha *= decay;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    double getMinDistSum(vector<vector<int>>& positions) {
        int n = positions.size();
        double x = 0, y = 0;
        for (auto& p : positions) {
            x += p[0];
            y += p[1];
        }
        x /= n;
        y /= n;
        double decay = 0.999;
        double eps = 1e-6;
        double alpha = 0.5;
        while (true) {
            double gradX = 0, gradY = 0;
            double dist = 0;
            for (auto& p : positions) {
                double a = x - p[0], b = y - p[1];
                double c = sqrt(a * a + b * b);
                gradX += a / (c + 1e-8);
                gradY += b / (c + 1e-8);
                dist += c;
            }
            double dx = gradX * alpha, dy = gradY * alpha;
            if (abs(dx) <= eps && abs(dy) <= eps) {
                return dist;
            }
            x -= dx;
            y -= dy;
            alpha *= decay;
        }
    }
};
```

#### Go

```go
func getMinDistSum(positions [][]int) float64 {
	n := len(positions)
	var x, y float64
	for _, p := range positions {
		x += float64(p[0])
		y += float64(p[1])
	}
	x /= float64(n)
	y /= float64(n)
	const decay float64 = 0.999
	const eps float64 = 1e-6
	var alpha float64 = 0.5
	for {
		var gradX, gradY float64
		var dist float64
		for _, p := range positions {
			a := x - float64(p[0])
			b := y - float64(p[1])
			c := math.Sqrt(a*a + b*b)
			gradX += a / (c + 1e-8)
			gradY += b / (c + 1e-8)
			dist += c
		}
		dx := gradX * alpha
		dy := gradY * alpha
		if math.Abs(dx) <= eps && math.Abs(dy) <= eps {
			return dist
		}
		x -= dx
		y -= dy
		alpha *= decay
	}
}
```

#### TypeScript

```ts
function getMinDistSum(positions: number[][]): number {
    const n = positions.length;
    let [x, y] = [0, 0];
    for (const [px, py] of positions) {
        x += px;
        y += py;
    }
    x /= n;
    y /= n;
    const decay = 0.999;
    const eps = 1e-6;
    let alpha = 0.5;
    while (true) {
        let [gradX, gradY] = [0, 0];
        let dist = 0;
        for (const [px, py] of positions) {
            const a = x - px;
            const b = y - py;
            const c = Math.sqrt(a * a + b * b);
            gradX += a / (c + 1e-8);
            gradY += b / (c + 1e-8);
            dist += c;
        }
        const dx = gradX * alpha;
        const dy = gradY * alpha;
        if (Math.abs(dx) <= eps && Math.abs(dy) <= eps) {
            return dist;
        }
        x -= dx;
        y -= dy;
        alpha *= decay;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
