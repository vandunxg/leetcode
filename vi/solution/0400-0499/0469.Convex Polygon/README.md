---
comments: true
difficulty: Medium
tags:
    - Geometry
    - Array
    - Math
    - Polygon
---

<!-- problem:start -->

# [469. Convex Polygon 🔒](https://leetcode.com/problems/convex-polygon)

[中文文档](/solution/0400-0499/0469.Convex%20Polygon/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các điểm <code>points</code> trên mặt phẳng <strong>X-Y</strong>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>. Nối các điểm theo thứ tự sẽ tạo thành một đa giác.</p>

<p>Trả về <code>true</code> nếu đa giác này <a href="http://en.wikipedia.org/wiki/Convex_polygon" target="_blank">lồi</a>, ngược lại trả về <code>false</code>.</p>

<p>Có thể giả định đa giác tạo bởi các điểm đã cho luôn là <a href="http://en.wikipedia.org/wiki/Simple_polygon" target="_blank">đa giác đơn</a>. Nói cách khác, tại mỗi đỉnh chỉ có đúng hai cạnh giao nhau; các cạnh không giao nhau ở vị trí nào khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0469.Convex%20Polygon/images/covpoly1-plane.jpg" style="width: 300px; height: 294px;" />
<pre>
<strong>Đầu vào:</strong> points = [[0,0],[0,5],[5,5],[5,0]]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0469.Convex%20Polygon/images/covpoly2-plane.jpg" style="width: 300px; height: 303px;" />
<pre>
<strong>Đầu vào:</strong> points = [[0,0],[0,10],[10,10],[10,0],[5,5]]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= points.length &lt;= 10<sup>4</sup></code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>Tất cả điểm đã cho đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trong đa giác lồi, hướng rẽ tại mọi đỉnh đều giống nhau. Chỉ cần kiểm tra hướng của từng bộ ba điểm; không cần dựng convex hull.
>
> Tại đỉnh $i$, tính $\overrightarrow{p_i p_{i+1}}\times \overrightarrow{p_i p_{i+2}}$. Nếu tích có hướng khác 0 và trái dấu với tích khác 0 trước đó thì đỉnh này tạo góc lõm. Tích bằng 0 (các điểm thẳng hàng) thì giữ nguyên hướng đã ghi nhận.
>
> Chỉ số quay vòng theo modulo $n$. Chỉ so sánh dấu khi có góc rẽ thực sự giúp các đỉnh thẳng hàng không làm phép kiểm tra thất bại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isConvex(self, points: List[List[int]]) -> bool:
        n = len(points)
        pre = cur = 0
        for i in range(n):
            x1 = points[(i + 1) % n][0] - points[i][0]
            y1 = points[(i + 1) % n][1] - points[i][1]
            x2 = points[(i + 2) % n][0] - points[i][0]
            y2 = points[(i + 2) % n][1] - points[i][1]
            cur = x1 * y2 - x2 * y1
            if cur != 0:
                if cur * pre < 0:
                    return False
                pre = cur
        return True
```

#### Java

```java
class Solution {
    public boolean isConvex(List<List<Integer>> points) {
        int n = points.size();
        long pre = 0, cur = 0;
        for (int i = 0; i < n; ++i) {
            var p1 = points.get(i);
            var p2 = points.get((i + 1) % n);
            var p3 = points.get((i + 2) % n);
            int x1 = p2.get(0) - p1.get(0);
            int y1 = p2.get(1) - p1.get(1);
            int x2 = p3.get(0) - p1.get(0);
            int y2 = p3.get(1) - p1.get(1);
            cur = x1 * y2 - x2 * y1;
            if (cur != 0) {
                if (cur * pre < 0) {
                    return false;
                }
                pre = cur;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isConvex(vector<vector<int>>& points) {
        int n = points.size();
        long long pre = 0, cur = 0;
        for (int i = 0; i < n; ++i) {
            int x1 = points[(i + 1) % n][0] - points[i][0];
            int y1 = points[(i + 1) % n][1] - points[i][1];
            int x2 = points[(i + 2) % n][0] - points[i][0];
            int y2 = points[(i + 2) % n][1] - points[i][1];
            cur = 1L * x1 * y2 - x2 * y1;
            if (cur != 0) {
                if (cur * pre < 0) {
                    return false;
                }
                pre = cur;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isConvex(points [][]int) bool {
	n := len(points)
	pre, cur := 0, 0
	for i := range points {
		x1 := points[(i+1)%n][0] - points[i][0]
		y1 := points[(i+1)%n][1] - points[i][1]
		x2 := points[(i+2)%n][0] - points[i][0]
		y2 := points[(i+2)%n][1] - points[i][1]
		cur = x1*y2 - x2*y1
		if cur != 0 {
			if cur*pre < 0 {
				return false
			}
			pre = cur
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
